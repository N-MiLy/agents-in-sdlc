---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメントの規約を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、作成したいコードだけでなく、その構成方法も記述できます。絞り込み機能を構築したので、Copilot が使用した指示を確認し、ドキュメントの規約を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない決定事項を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化します。関連するコードが変わったら、コメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの慣例に沿ってタスクを実行できるよう、適切な情報を提供します。

## 指示ファイル

カスタム指示では、コンテキストや希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイル形式、タスクのガイドラインを記載します。

> [!NOTE]
> その他の指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

プロジェクトには、あらかじめ指示ファイルが用意されています。変更を加える前に、既存の内容と変更の影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、リポジトリでの Copilot とのやり取りすべてに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。このフィールドでは、指示を適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、参考にする例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。新しく生成される TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しのセクションを探します。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、会話を終了して絞り込み機能の会話を再開します。機能のコンテキストを保ったまま、新しい指示を適用できます。

1. 絞り込み機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチ上のリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、絞り込み機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に新しいガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用できる quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
