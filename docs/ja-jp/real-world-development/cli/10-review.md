---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、あらかじめ用意された絞り込み機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み機能の Issue から始め、Plan モードで方針を定義し、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- 絞り込み機能の変更全体をレビューし、絞り込み機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を作成します。

1. **星評価**: 既存の `starRating` と未評価状態を、ゲームカードに明示して表示します。
2. **絞り込みと品質のワークフロー**: 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

絞り込み機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果と要件およびテスト カバレッジのレビューをまとめて PR に反映しました。

追加したテストは、実際に存在する不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと証拠をレビューし、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し行うタスクを説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル**を選択します。構築前に方針を検討するときは **Plan**、範囲を絞った変更を自分で確認しながら進めるときは **Interactive**、範囲が明確なタスクを実行するときは **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。構築するもの、その理由、方法を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを学びました。次は、ほかの CLI 機能も試してみましょう。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を説明し、別の視点から考えます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。製品コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。学んだことをチームと共有し、チームからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
