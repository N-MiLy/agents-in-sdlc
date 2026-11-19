---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを管理してもらいます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。ワークショップを簡潔にするため、絞り込み機能と品質ワークフローの作業は、同じ会話とブランチで進め、この PR にまとめています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の証拠を確認します。
- 絞り込み機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能のワークフローを通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど) に対処します。その後、リポジトリで許可されていれば、GitHub の自動マージを有効にします。

これまでは、自分で **プルリクエストをマージ**を選択しました。Agent Merge にその作業を任せることができます。有効にする前に、作業をレビューして、マージできる状態か判断してください。

## Agent Merge で PR を管理する

コードがそろったら、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して、新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーション プロセスを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の証拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、[コンテキスト、モデル、共有、クラウドへの任意の委任に使える Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
