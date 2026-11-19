---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、そのプルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz をデプロイして、誰でもアクセスできる場所に公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは自分の役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages のデプロイを設定するよう依頼します。
- ワークフローとプルリクエストをマージ前にレビューします。
- デプロイの状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ簡単に試せる方法がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーや実行時の依存関係を追加せずに、公開 URL から利用できるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストの作成によってコードがコミットおよびプッシュされることがあります。プロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**すべて承認**に設定されている場合、個別の確認プロンプトが表示されるとは限りません。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. README は、リポジトリにまだ存在しない場合に限って追加されていることを確認します。
2. ビルドとデプロイの手順、トリガー、権限など、プルリクエストの変更をレビューします。
3. ワークフローが、マージされていないプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 必要な修正を依頼し、更新後の差分をレビューします。マージを許可する前に、必要なプルリクエストのチェックが合格するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送信すると、エージェントにレビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開を許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作に別の確認プロンプトが表示されないことがあります。

`<PR_NUMBER>` をレビュー済みのプルリクエスト番号に置き換えてから、次のプロンプトを別途送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図: マージ済みの space-quiz プルリクエスト、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、作業が完了したと判断しないでください。

## まとめと次のステップ

Space Quiz をサイトとして公開し、エージェントの報告だけに頼らず、デプロイを確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みましょう。

[next-lesson]: ../13-review/
