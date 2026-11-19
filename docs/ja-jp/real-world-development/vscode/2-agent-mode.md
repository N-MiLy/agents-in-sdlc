---
title: "演習 2 - Copilot エージェント モードで新しい機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの操作が必要です。開発者は通常、必要なファイルをすべて探し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを見つけて、さらに更新するという作業を繰り返します。

このような作業に役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE 内で、より自律的に作業するように設計されています。開発者と同じように、既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに設計されています。エージェント モードは IDE 内でタスクを実行し、素早くやり取りしながら進められます。cloud agent はチームの同僚のように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルにわたって連携した変更を行います。
- GitHub Copilot エージェント モードで UI とデータ層にまたがる新機能を実装します。
- 変更をコードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。ここでは Copilot エージェント モードを使って、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリで構成され、ビルド時に静的 HTML としてページを生成します。ページはフロントマターから Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace 上で実行します。

1. Codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace を使用している場合、Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を掲載した詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに、Issue のバックログも自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルにまたがる変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、スムーズな体験にすることが目的です。ただし、LLM の動作は確定的ではないため、ワークショップの記述と異なる提案が表示されることがあります。これは正常で、想定される動作です。

> [!NOTE]
> LLM の動作は確定的ではないため、Copilot が別の MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを送信して、バックログの Issue を Copilot に確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **続行**を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は、ユーザーに最も役立つと考える項目を優先して表示します。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、Copilot にコンテキストを提供する指示ファイルを確認しましょう。Web サイトに機能を追加する方法に関するコンテキストを記載した [ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則
   - ファイルの種類ごとの指示ファイルへのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、複雑な内容を小さく管理しやすい単位に分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、コードベースにマージする前にテストを実行し、合格する必要があります。Copilot エージェント モードにこれらのタスクを依頼しましょう。

1. Copilot との現在の会話を続けるか、**新しいチャット**を選択して新しい会話を始めます。
2. **コンテキストの追加**、**指示**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中は、コード エディターに **保持**と**元に戻す**が表示される場合があります。Copilot が完了すると、すべての変更に対して **保持**または**元に戻す**を選べるようになるため、作業中に選択する必要はありません。

7. Copilot に促されたら、**続行**を選択してテストを実行します。

   ![Copilot Chat ペインに表示された、テスト実行の確認ダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーが見つからなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードの Chat セッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **保持**を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次の内容を確認しました。

- GitHub Copilot エージェント モードで UI とデータ層にまたがる新機能を実装できます。
- Copilot エージェント モードはプロジェクトを調べ、関連ファイルを特定し、連携した変更を行えます。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューします。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にサイトへのページネーション追加を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、値を固定するかなど、ページネーションの方法を検討します。
- 希望どおりの実装ができるよう、必要なガイダンスを含めてプロンプトを作成します。
- GitHub Copilot に変更やコンテキストを伝え、何度か反復する必要がある場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
