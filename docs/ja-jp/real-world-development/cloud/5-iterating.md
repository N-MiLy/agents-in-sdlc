---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが改善できる関連ゲーム機能を構築し、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝え、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視なしでタスクを実行するため、安全性を保つためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限られます。
- cloud agent は GitHub Actions 内で動作し、作業には専用の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーなど、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** のフラグが付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![プルリクエストで変更されたファイルの一覧を表示する Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。新しい TSDoc コメントやその他のドキュメントが含まれます。正確な変更内容は異なります。

    変更を確認しながら、TSDoc コメント、TypeScript の慣例、コメント ヘッダーを探します。これらは、演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了までに数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストでやり取りするときは、一方的に作業を受け取るだけではありません。チーム メンバーに依頼するように、プルリクエストやコードのインライン コメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず依頼すべき内容はありません。次のような変更を提案できます。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例、またはコードベースに追加したいドキュメントの提案を使って、生成されたドキュメントの変更をコメントで依頼します。他のユーザーと同じように **@copilot** をタグ付けします。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る Back to pull request ボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

満足するまで反復して改善できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、`main` ブランチにマージします。

![ドラフト PR をレビュー可能な状態にする Convert PR to ready ボタン](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR に戻りましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了までに数分かかることがあります。
9. **オプション:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (Copilot が作成したブランチ名、たとえば **copilot/fix-8** に `<branch-name>` を置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を構築しました。前と同じように、Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数の変更や、コメント ヘッダーおよび TSDoc コメントの追加を依頼できます (この Issue はカスタム指示の更新**前**に割り当てたことを思い出してください)。前と同じように、**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 の mission control で追加を依頼したライト モードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** のフラグが付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装をレビューし、特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持するためのローカル ストレージの使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 一部のワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン (Approve and run workflows)](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了までに数分かかることがあります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (Copilot が作成したブランチ名に `<branch-name>` を置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントが、アクセシビリティのベスト プラクティスに従うよう Copilot をガイドしたことを確認します。アクセシビリティ上の懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** をタグ付けし、更新を依頼できます。

## オプションの演習 — 委任を続ける

実際のバックログ項目をエージェントに任せ、自分は別の作業に取り組むと、cloud agent を効果的に活用できます。この習慣を身につけるために、リポジトリに追加の Issue を作成し、Copilot に割り当ててみましょう。次のような作業を依頼できます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次の操作を行いました。

- cloud agent の出力への影響を確認できるよう、このリポジトリに用意されたカスタム指示ファイルを**確認しました**。
- Issue を Copilot cloud agent に**割り当て**、環境をセットアップして計画し、非同期に作業する様子を確認しました。
- アクセシビリティ向けの**カスタム エージェントを作成して使用**し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot エージェント ページを mission control として使い、アクセシビリティ セッションの途中で監視し、方向を調整しました。
- Copilot のプルリクエストを**レビューして反復的に改善**し、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学ぶ場合は、他のハーネスで今回の内容をさらに実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタム エージェントを作成します。
- 別のスタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
