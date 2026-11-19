---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se houver alguma, não enfrentam dívidas técnicas. Elas podem incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de maneira autônoma. Quando termina o trabalho, ele cria um pull request de rascunho para que uma pessoa desenvolvedora possa revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviços inicialmente contratados para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes de um jogo deve exibir jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Isso é um bloqueio no momento, mas há outras issues com prioridade maior.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e que são ótimas candidatas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades. Depois, você pode voltar para revisar o trabalho e confirmar que está tudo certo.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa da equipe, há várias maneiras de iniciar uma tarefa, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode inspecionar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: não é possível pular a etapa de raciocínio. Até tarefas aparentemente simples podem se tornar complexas quando examinadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas do agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os princípios fundamentais do desenvolvimento de software não mudam com a inclusão de IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se ainda não fez, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório sem alterá-los para que você possa identificar a influência deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace já iniciado:

- `.github/copilot-instructions.md`: revise a seção **Code standards**, principalmente as expectativas para as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita estas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções. Voltaremos a isso ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Desenvolver código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto, independentemente de quem esteja realizando o trabalho. O mesmo se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de trabalho especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para executar antes do início do trabalho. Isso disponibiliza as ferramentas de desenvolvimento e dependências necessárias. Esse fluxo já foi configurado para facilitar o laboratório e o aprendizado. Ele fornece Node.js, dependências do projeto, binários do navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo: Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que as compilações e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`, executado pelo GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se o fluxo funciona sem precisar esperar que o Copilot o execute.

## Adicione documentação

Embora todo mundo reconheça a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma não ser resolvido, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito eficaz na criação de documentação, então esta é uma issue ideal para atribuir ao agente de nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o campo **Title** como `Code lacks documentation`.
6. Defina o campo **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se o PR ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Após alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão realizadas.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: ele explora o código e entende o estado atual, pausa para avaliar e definir um plano e, por fim, cria o código.

Esse processo provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção para o desenvolvimento de funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho para que você possa se concentrar em um conjunto de tarefas enquanto ele cuida de outro. Mesmo que a inclusão de uma seção de jogos relacionados não leve muito tempo para a equipe de design, você ainda poderia usar esse tempo para outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o campo **Title** como `Show related games on the game details page`.
6. Defina o campo **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um conjunto de 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. O agente de nuvem funciona de maneira semelhante a uma pessoa engenheira de software, então não é necessário acompanhá-lo ativamente. Basta revisar o resultado depois que o trabalho for concluído. Vamos passar à criação e ao uso de agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, é possível atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Ele pode ajudar a resolver dívidas técnicas, criar funcionalidades ou migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalização do ambiente de geração de código.
- garantia de que as operações sejam realizadas com segurança.
- importância de issues bem delimitadas.
- atribuição de issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e as instruções personalizadas que vimos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
