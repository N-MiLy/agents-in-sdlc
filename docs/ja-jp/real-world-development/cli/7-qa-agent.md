---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーから絞り込み機能を確認しました。次は、明確な QA プロセスを定義したカスタム エージェントで、これらの機能を組み合わせます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用できる品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、絞り込み機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する Copilot の専門家向け構成です。プロファイルには、エージェントの目的、指示、使用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルには、繰り返し実行できるチェックが含まれます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件の評価と結果の報告に、これらの機能をどう使うかを Copilot に伝えます。ほかの機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。プロファイルでは、QA が実施するチェックと、その作業範囲の両方を定義します。

1. Codespace に戻り、絞り込み機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のように依頼してカスタム エージェントを作成します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、プロファイルを確認し、意図した QA ワークフローと作業範囲が反映されていることを確かめます。これにより、検証だけを行いたいときに、指示が不足したエージェントや範囲が広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、設定するとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの責任や境界がプロファイルに含まれていない場合は、続ける前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じ絞り込み機能の会話を再開し、QA を選択します。Issue と計画時の決定事項を引き続き参照できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。エージェントは利用できるため、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、エージェントのレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、絞り込み機能の Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートをレビューできる状態になりました。次は、[機能 PR にまとめ、Agent Merge を使用し][next-lesson]ます。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
