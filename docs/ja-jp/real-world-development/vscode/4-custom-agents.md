---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に実行するための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダーか、グローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、機能上の重なりがあります。どちらも主に Markdown ファイルで定義し、操作方法を AI に指示します。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**だということです。

カスタム エージェントは専用のコンテキスト ウィンドウを持ち、スキルや他のエージェントを調整しながら作業します。このラボでは、アクセシビリティ カスタム エージェントがアクセシビリティ ガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフロー スキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントを作成する唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせて試し、反復してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのようにガイドするか理解するため、内容を確認しましょう。

まず Codespace に戻ります。

ターミナルを開き、`main` からアクセシビリティ作業用の新しいブランチに切り替えます。演習 3 の絞り込み機能の PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

続いて、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担当する際の基本的な役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と間違い

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義したすべてのカスタム エージェントが Copilot Chat ビューの下部にあるエージェント ドロップダウンに表示されます。カスタム エージェントを選ぶと、そのエージェントの指示とツールに対象を絞ったチャット セッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**新しいチャット**を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューのエージェント ドロップダウンから、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャット ビューの下部にあるエージェント ドロップダウンを選択します。現在のエージェント (既定では **default**) が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調べ、変更を提案して、プロジェクトのファイルに適用します。各編集はファイル パスとレビューできる差分とともに、チャット内に表示されます。
6. エージェントがファイルを編集すると、**変更されたファイル**の表示が更新され、エージェントが変更した作業範囲を確認できます。

> [!NOTE]
> この処理には数分かかります。Copilot はリポジトリに実際の変更を加えます。作業中に Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューし、方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot がより効果的に作業できるようガイドできます。

次の概念を確認しました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は、エージェントの作業を[監視して方向を調整し][next-lesson]、変更を確認しながら、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
