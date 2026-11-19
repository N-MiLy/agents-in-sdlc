---
title: "Exercício 2 - Adicione uma funcionalidade com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e a execução de operações como testes. O fluxo de trabalho de quem desenvolve normalmente envolve localizar todos os arquivos necessários, fazer alterações, executar testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... E a lista continua.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi desenvolvido para agir com mais autonomia no IDE. Ele trabalha de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona como uma pessoa da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena as alterações.
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, com o modo de agente do Copilot, concluirá o trabalho restante na camada de dados, na interface e nos testes.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender o que ele faz atualmente.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM. Não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro, limitados ao escopo necessário e usando APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao Codespace. Após o exercício anterior, você deverá estar na branch `main`.
2. Abra uma nova janela do terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner com a URL, semelhante ao exemplo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se for aberta uma caixa de diálogo solicitando que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, ao selecionar o link para a URL `localhost` no terminal, você será redirecionado automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore sua funcionalidade. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa especifica essas opções quando necessário.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. Ao criar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot para exibir esses itens.

1. No menu suspenso de agentes da visualização Chat, selecione **Agent**. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e chama ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não indicam preferência por um modelo específico. Usamos o Claude Sonnet 4.5 ao criar o workshop e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e ofereçam uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie o prompt a seguir ao Copilot para consultar o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de iniciar o agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios para o design de componentes, a testabilidade e a acessibilidade.
   - links para arquivos de instruções específicos dos tipos de arquivo, como:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Assim, você pode reutilizar orientações, dividir conjuntos complexos de instruções em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes da mesclagem. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual com o Copilot ou selecione **New Chat** para iniciar uma nova.
2. Selecione **Add Context**, **Instructions** e o arquivo `ui`.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog usando o prompt a seguir:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados ao recurso. Ele deverá encontrar os auxiliares da camada de dados, a interface e os testes. Em seguida, começará a alterar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os controles **Keep** e **Undo** no editor. Quando o Copilot terminar, você terá a opção **Keep** ou **Undo** para todas as alterações; não é necessário selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. Não se preocupe. O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena as alterações.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração: implemente a paginação

À medida que a lista de jogos crescer, será necessário habilitar a paginação. Com base no que aprendeu neste exercício, peça ao Copilot que atualize o site para incluir paginação. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis.
- decidir como implementar a paginação: permitir que a pessoa usuária selecione o tamanho da página ou definir um valor fixo.
- incluir no prompt as orientações necessárias para que o Copilot implemente a solução desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
