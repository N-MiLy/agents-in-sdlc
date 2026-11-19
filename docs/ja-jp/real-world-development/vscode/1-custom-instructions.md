---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。コンテキストを Copilot と共有する方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、絞り込み機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されたコードが新しい標準に従うことを確認します。
- 次の演習で Copilot が更新されたガイダンスを使用できるよう、指示の更新と絞り込み機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot の動作は確定的ではありません。この演習では、出力の文字単位の一致ではなく、指示を更新した後の動作の**傾向**を確認します。

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

リポジトリに用意された指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの複数の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスがあります。これらは、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドでは、指示を適用するファイルを決めるリポジトリ ルートからの glob を指定します。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これらを使うと、大きな指示を小さな再利用可能ファイルに分け、コード生成の参考例を Copilot に示せます。ここにあるパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はありません。次の手順で、TSDoc コメントとファイル コメント ヘッダーの規則を追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の効果を確認するには、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。多くの場合、ヘルパー ファイルとテストが変更されます。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の慣例に基づいています。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確定的ではないため、指示がなくてもドキュメント コメントが追加される場合があります。その場合も問題ありません。指示を更新した後に動作の一貫性が高まることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**新しいチャット**を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot が適切なファイルに集中するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot をガイドし、次の演習でも利用するコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のアセットと同じようにソース管理で管理します。変更をコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを Copilot に使ってもらいましょう。Issue を検索し、それをクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> この演習では初めて Copilot に GitHub で操作してもらいます。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに接続されているため、設定は不要です。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従って許可してください。後の演習では、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルを更新する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、絞り込み機能の基盤となる発行元ヘルパーの追加の両方を説明するようにします。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot は Issue を検索し、PR を作成します。
5. Copilot が代わりに GitHub 操作を実行することを **許可**するよう求められたら、コマンドを確認して適切に **許可**します。
6. PR が作成されたら、次のプロンプトを使ってマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代わりに GitHub 操作とシェル コマンドを実行することを **許可**するよう求められたら、コマンドを確認して適切に **許可**します。

これで GitHub Copilot の支援を受けて、プルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- コード生成プロンプトを送信し、*既存の*指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェント モード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
