---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent に、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。その 1 つが GitHub.com の[エージェント ページ][agents-page]です。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。エージェントの操作を調べたり、実行中のセッションに指示を追加して方向を調整したりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加機能を依頼し、作業の方向を調整します。

## シナリオ

ハイ コントラスト モードの作成をエージェントに割り当てた後、チームはライト モードも追加するとよいと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに行っているため、ライト モードも含めるのが自然です。エージェントがハイ コントラストに加えてライト モードも実装するよう、作業の方向を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインでタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイ コントラスト モードの追加
3. 実行中のタスクを 1 つ選択し、Copilot が実行した操作を確認します。次のような作業が含まれます。
   - リポジトリからコードをチェックアウトする
   - Copilot の作業環境を作成する
   - MCP サーバーをセットアップする
   - 割り当てられたタスクを完了するためのさまざまな作業を行う

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。追加で監視するために、PR と変更されたファイルを確認できます。

## cloud agent の作業の方向を調整する

実行中のタスクを確認できました。次は、エージェントがハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの追加を示すセッションを選択します。Copilot が付けた名前や作業の状態によって、正確なタイトルは異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Copilot の作業中にアクティブなセッションの方向を調整する**ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページの cloud agent タスク。作業中のアクティブなセッションの方向を調整するダイアログが強調表示されています](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れることを確認します。

## Copilot に作業を任せる

前と同じように、Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼をワークフローに取り込みます。

これまでに学んだことを振り返りながら、しばらく待ちましょう。完了までには数分かかります。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視し、ガイドする中心的な場所である Copilot エージェント ページを確認しました。この mission control では、実行中および完了済みのタスクを追跡し、実施された作業を確認し、進行中のタスクの範囲やガイダンスを変更できます。

次の概念を確認しました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼して、作業の方向を変更しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成したプルリクエストを[反復的に改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
