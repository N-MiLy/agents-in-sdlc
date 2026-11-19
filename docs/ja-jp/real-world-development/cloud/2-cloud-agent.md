---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした Issue の重要性
- Copilot への Issue の割り当て

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを適切に整備しませんでした。具体的には、ドキュメントがまったくありません。最初の対応として、アプリケーションのエクスポートされたすべての関数に TSDoc のコメントを追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。まだ仕上げられた実装は必要なく、UX の受け入れテストに使える状態にしたいと考えています。現在は作業の妨げになっていますが、より優先度の高い Issue がほかにあります。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期に作業を進めてもらえます。その間、開発者は別のタスクに集中し、後から Copilot の作業をレビューして期待どおりであることを確認できます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手順を省略できるわけではありません。一見簡単そうな操作にも、詳しく調べると複雑さが見つかります。[Copilot cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、作業を調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示のハンズオンをすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認したはずです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認しておきましょう。後で影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の慣例と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるときは、作成されたプルリクエストで TSDoc コメント、TypeScript の慣例、コメント ヘッダーを確認します。後の演習で PR をレビューするときに、もう一度この点を取り上げます。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードの作成には通常、特定の環境とセットアップが必要です。開発者に近い方法でタスクを実行する Copilot に作業を割り当てる場合も同様です。

cloud agent は作業時の環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前の環境をカスタマイズできます。必要な開発ツールと依存関係を利用できるようになります。ワークショップの流れを円滑にして学習に集中できるよう、この環境は事前に構成済みです。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、ワークフローの成功をテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、ドキュメントがなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **新しい Issue** を選択して、新しい Issue のダイアログを開きます。
4. **空白の Issue** を選択して、新しい Issue を作成します。
5. **タイトル**に `Code lacks documentation` と入力します。
6. **説明**に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **作成**を選択して Issue を作成します。
8. 右側の **Copilot に割り当て**を選択して、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **割り当て**を選択します。

  ![Copilot の割り当ての詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に PR が表示されない場合は、1、2 分待ってからブラウザーを更新します。
12. 数分後に、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は、前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、最適な方法を検討しています。

13. リストと、実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **セッションを表示**ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、考えながら適切な計画を決め、コードを作成しています。

完了までには数分かかります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行し、開発者が別の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを Copilot cloud agent に割り当てます。その間、アプリケーションに機能を追加するコードの作成に取り組みましょう。

## ゲームの詳細ページに関連ゲームを追加する

前に説明したように、GitHub Copilot cloud agent の大きな利点は作業を分けられることです。デザイン チームのために関連ゲーム セクションを追加する作業に長い時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **新しい Issue** を選択して、新しい Issue のダイアログを開きます。
4. **空白の Issue** を選択します。
5. **タイトル**に `Show related games on the game details page` と入力します。
6. **説明**に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **作成**を選択して Issue を作成します。
8. 右側の **Copilot に割り当て**を選択して、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **割り当て**を選択します。

少しすると、Issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めたことを示しています。

![目の絵文字で Copilot が Issue に取り組んでいることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は開発者に近い方法で作業するため、積極的に監視する必要はなく、完了後にレビューすればよいでしょう。ここからは、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期に作業を進めてもらえます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 範囲を明確にした Issue の重要性
- Copilot への Issue の割り当て

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用を確認できます。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も利用でき、前のモジュールで確認したカスタム指示も読み込みます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
