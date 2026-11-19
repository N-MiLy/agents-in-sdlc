---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。Codespace では指示ファイルを編集し、cloud agent が作成した作業をレビューできます。

## ラボ用リポジトリをセットアップする

コードを作成するためのリポジトリのコピーを作ります。[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスにはラボに必要なファイルがすべて含まれます。演習中は、このリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリ ページの **このテンプレートを使用**を選択し、**新しいリポジトリを作成**を選択して、自分用のコピーを作成します。

    ![「このテンプレートを使用」ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

    ![リポジトリ テンプレートの設定を入力する](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモしておきます。ラボの後半で使用します。

## Codespace を作成する

続いて、Codespace を使ってラボの演習を行います。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの記述、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えた IDE を利用できます。

1. 作成したばかりのリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに短時間で済みます。作成を待つ間、次に説明する GitHub Copilot の機能を試してみましょう。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらの環境にも、スムーズに作業するための前提条件がインストールされています。ローカルで作業する場合は、複製したリポジトリを VS Code で開き、プロンプトが表示されたら **コンテナーで再度開く**を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

おめでとうございます。ラボ用リポジトリのコピーを作成できました。また、Copilot cloud agent と一緒に作業するときに使う Codespace の作成を開始しました。

## 次のステップ

cloud agent が従うカスタム指示を追加しましょう。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
