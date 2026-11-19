---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使ってガイダンスを提供すると、Copilot は何をするかだけでなく、どのように実行するかも理解できます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、他の優れた開発チームと同じように、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードで、現代的な見た目にします。
- コードのドキュメントとして TSDoc のコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣例に沿ってタスクを実行できるよう、適切な情報を提供します。

### カスタム指示

カスタム指示では、コンテキストや希望を Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。関連性の高い提案やコード スニペットを得るために、コーディング規約、ライブラリ、コードに含めるコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。Copilot へのチャットや CLI のリクエストの大半に関係する、プロジェクト レベルの情報を記載します。使用する技術スタック、開発するものの概要、ベスト プラクティス、全体に適用するガイダンスなどが該当します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類向けに作成できます。TypeScript や Astro などの特定の言語、UI コンポーネントの作成やユニット テストの追加などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方をコード生成に使います。
>
> さらに、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準に基づく指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプル プロジェクトの例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、開発するもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイルの種類 (ユニット テスト、Astro コンポーネント、データ層など) や特定のタスク向けの指示を記載します。
- 自然な言葉を使い、ガイダンスを分かりやすくします。コードのよい例と悪い例を示します。

指示ファイルの作成にも AI の利用にも、唯一の方法はありません。プロジェクトに合った方法を、試しながら見つけましょう。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトには、十分な指示ファイルが必要です。このプロジェクトの指示を確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルが用意されていることが分かります。
>
> Copilot に指示ファイルを生成してもらうこともできます。方法は利用する画面によって異なります (たとえば VS Code では **チャットを構成 → エージェントの指示を生成**、Copilot CLI では `/init`)。その画面に対応するレッスンで、関連する場合に説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] リポジトリを確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスがあります。これらは、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示を適用するファイルを決めるリポジトリ ルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これらを使うと、大きな指示を小さな再利用可能ファイルに分け、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はありません。次の手順で、TSDoc コメントとファイル コメント ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、コード内のドキュメントを TSDoc コメントとファイル レベルのコメント ヘッダーとして記載するという、プロジェクト全体のルールだけです。cloud agent は、このハーネスの後半で割り当てる Issue に取り組むときに、このルールを読み込みます。

編集の前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読みますが、変更をきれいにコミットするためにブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えるためのファイルです。コード提案の改善に役立つよう、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。TSDoc コメントとファイル コメント ヘッダーを Copilot に追加させます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` として作成することもできます。今回追加したドキュメント コメントの規則のような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチの指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue に割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。ここでは、実際のプロジェクトで行う方法に近づけるため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストを確認するときに、その標準が適用されていることを確認できます。

次は、[最初の Issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
