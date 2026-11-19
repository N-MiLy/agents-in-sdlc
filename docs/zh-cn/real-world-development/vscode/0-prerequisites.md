---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要做好准备。你将创建自己的 Tailspin Toys 存储库副本、启动一个用于开发的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已启动并正常运行。

## 设置实验存储库

要创建用于编写代码的存储库副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的全部文件，你将在接下来的练习中使用它。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择**使用此模板**按钮，创建自己的存储库副本。然后选择**创建新存储库**。

    ![“使用此模板”按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），之后的实验步骤中会用到它。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题。整个研讨会中都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来，你将使用 Codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

创建 Codespace 需要几分钟，不过仍比手动安装所有服务快得多！你可以利用这段时间探索 GitHub Copilot 的其他功能，接下来我们会介绍这些内容。

> [!CAUTION]
> 之后的练习中你会再次使用这个 Codespace。现在请先让它保持打开状态，并保留在浏览器选项卡中。

> [!NOTE]
> 本研讨会既可在 Codespace 中运行，也可在本地[开发容器][dev-containers]中运行。两种方式都会安装顺利完成操作所需的所有先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库；看到提示时选择**在容器中重新打开**，VS Code 会构建与 Codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。若你使用的是 GitHub Codespace，通常已经安装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat** 并选择**安装**。

安装扩展后，可能需要使用 GitHub 帐户进行身份验证才能启用它。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。这样应该会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示你登录 GitHub 帐户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口应该会显示出来。

## 总结

恭喜你，已创建实验存储库的副本！你也启动了 Codespace 的创建流程，接下来会使用它开始编写代码。

## 下一步

现在开始使用 Copilot。继续学习[练习 1 - 自定义指令][next-lesson]，向 Copilot 介绍项目规范。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
