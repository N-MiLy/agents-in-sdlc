---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュ コマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキスト ウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドで状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分割されたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI は、ウィンドウが埋まるとコンテキストを自動的に圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始めたい場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用量に関する情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を互いに向上できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. このセッションを共有する場合は、目的に合った方法を選びます。たとえば `/share file` は、ローカルに Markdown としてエクスポートします。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## オプション: 作業の拡張や委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プルリクエストを開くことがあります。

これらのコマンドは worktree やリモート作業を追加で作成することがあるため、任意の内容です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、CLI の構成、セッションの共有、内部動作の確認ができます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュ コマンドがあり、さらに多くの機能を試せます。次は、[学んだ内容を振り返り][next-lesson]、学習を続ける方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
