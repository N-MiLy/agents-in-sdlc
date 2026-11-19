---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

すでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が追加の自動化を利用できるようにすることも必要です。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を理解します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

ユニット テストとエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを操作できるようにすると、変更が期待どおりに動作することを確認し、作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。最新の情報にアクセスしたり、ユーザーに代わって操作を行ったりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP レジストリ][mcp-registry]を使うと、エコシステム内のサーバーを見つけ、貢献できます。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。使用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーが利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるように追加します。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI で設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認してもらう

これまでは機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に伝えます。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを見つけて利用するため、明示的に指示する必要はありません。ただし、使用するツールが分かっている場合は、指示しておくとよいでしょう。結果の一貫性を高め、トークンを節約できます。

2. ブラウザーで機能が動作することを確認するため、Copilot が実行する手順を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

おめでとうございます。Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認できました。このレッスンでは、次の操作を行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、[スキルとブラウザー ツールを組み合わせた QA カスタム エージェントを作成し][next-lesson]、専門家の役割を設定します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
