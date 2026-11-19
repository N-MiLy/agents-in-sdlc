---
title: "演習 3 - カスタム エージェント"
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

カスタム エージェントを使って、次の内容を確認します。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに作成されています。Copilot をどのようにガイドするか理解するため、内容を確認しましょう。

Codespace に戻り、アクセシビリティ カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担当する際の基本的な役割
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題と間違い

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを管理する場所です。Copilot cloud agent にタスクを割り当てて監視し、実行中のセッションに追加のガイダンスを提供できます。まず、ハイ コントラスト モードの作成を Copilot に依頼しましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **新しい Issue** を選択して、新しい Issue のダイアログを開きます。
4. **空白の Issue** を選択して、新しい Issue を作成します。
5. **タイトル**に `Add high contrast mode to website` と入力します。
6. **説明**に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **作成**を選択して Issue を作成します。
8. 右側の **Copilot に割り当て**を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面。カスタム エージェントとアクセシビリティが強調表示されています](../../../_images/ex5-select-custom-agent.png)

10. **割り当て**を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである、GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot がより効果的に作業できるようガイドできます。

次の概念を確認しました。

- カスタム エージェントの定義方法
- タスクをカスタム エージェントに割り当てる方法

Copilot がハイ コントラスト モードを実装している間に、mission control から[エージェント セッションを監視して方向を調整する][next-lesson]方法を確認しましょう。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを利用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
