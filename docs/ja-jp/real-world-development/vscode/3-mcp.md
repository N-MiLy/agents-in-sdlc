---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を構築しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーが新しいツールで Copilot を拡張する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーから絞り込み機能を手動テストするよう依頼します。
- ブランチを公開し、絞り込み機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エディターの外部に接続できると、エージェント モードの機能は大きく広がります。Model Context Protocol (MCP) は Copilot が外部に接続するための標準的な方法で、エージェントが外部のツールやサービスと通信できるようにします。

![エージェント モードの内部と、MCP サーバーや VS Code 拡張機能が提供するツールを含めた、コンテキスト、LLM、ツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を通じてリソースを使い、最新の情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

これらのツールとリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールと AI エージェント間の通信を管理します。各 MCP サーバーは、AI エージェントが使える異なるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントは、リポジトリの作成、更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースにアクセスできる MCP サーバーはほかにもあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)を使うと、エコシステム内のサーバーを見つけ、貢献できます。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用前にソース コードを注意深く確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密性の高いリソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot はバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能をテストするために必要なサーバーです。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して、自分のコンピューター上で起動します。`--headless` フラグは Playwright を画面のないブラウザーで実行するよう指定します。Codespace 内にはブラウザーを表示するデスクトップ環境がないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright をすでに使っているため、必要なブラウザーは通常インストール済みです。後で Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してもう一度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて必要としたとき、VS Code がサーバーを起動して信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、サーバーを起動できるよう信頼することを選択します。

手動で起動する必要はありません。スキップした場合は、次の手順で Copilot がツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。自分でアプリを操作しなくても、エージェントがページを開き、移動し、フィルターを適用して、結果を読み上げ、確認した内容を要約できます。会話を離れずに機能が期待どおりに動作することを確認する手軽な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、エージェントは支援技術と同様に、ボタン、リンク、リスト項目などの構造化されラベルの付いた要素を判断します。簡単な機能チェックが、アクセシビリティの簡易確認にもなります。

サーバーを接続し、アプリを実行したら、Copilot に絞り込み機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して、結果を報告します。Issue の受け入れ条件に照らして要約を読みます。問題がある場合は、プルリクエストを開く前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **ソース管理**パネルに移動し、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容が表示されたソース管理パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **公開**を選択して、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成を依頼しましょう。関連する Issue を見つけ、Issue に関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**新しいチャット**を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるよう、エージェント ドロップダウンから **Agent** が選択されていることを確認します。
3. 次のように Copilot に依頼して、PR を作成します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行うために **続行**が必要な場合は選択します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。画面の指示に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが新しいツールで Copilot を拡張する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、Copilot にブラウザーで絞り込み機能を手動テストしてもらいました。
- ブランチを公開し、絞り込み機能のプルリクエストを作成しました。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成し][next-lesson]ましょう。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
