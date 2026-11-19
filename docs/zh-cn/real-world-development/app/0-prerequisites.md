---
title: "第 0 课 - 先决条件"
description: "为 GitHub Copilot app 课程做好准备：安装 Tailspin Toys 项目所需的 Node.js，并从模板创建自己的存储库副本。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app 是一款桌面应用，也是你使用 Copilot 和 GitHub 的中心入口。它可以快速访问议题和拉取请求，当然也能让你使用 GitHub Copilot 构建项目。本研讨会中，你将在本地使用 GitHub Copilot app 更新基于 Astro 的 Tailspin Toys 应用程序。开始前，先确认本地已安装 Node.js，然后再安装 Copilot app。

在本课中，你将：

- 安装 Node.js，以便在计算机上运行项目测试。
- 使用模板创建 Tailspin Toys 项目的副本。

## 安装 Node.js

多节课程会要求智能体构建功能并在本地运行 Tailspin Toys 测试套件，这需要安装项目唯一需要的运行时——**[Node.js][nodejs]**。请安装当前的 **LTS** 版本。

所有平台上最简单的安装方式都是使用官方安装程序：

1. 在操作系统中打开终端，例如 Windows Terminal、macOS 终端或你常用的终端。
2. 运行以下命令，检查已安装的 Node.js 版本：

    ```shell
    node --version
    npm --version
    ```

3. 如果两个命令都能成功运行，且 Node.js 满足项目 README 和 `package.json` 中的要求，可以跳到下一部分。

> [!TIP]
> 只有在尚未安装 Node.js 或需要更新时，才需要完成以下步骤。

4. 打开 [Node.js 下载页面][node-download]。
5. 下载适用于操作系统的 **LTS** 版本。
6. 运行安装程序并接受默认选项。在 Windows 上，请保持选中 **Add to PATH** 选项。
7. 安装完成后，打开一个新的终端窗口。
8. 在新终端窗口中运行以下命令，确认安装成功：

    ```bash
    node --version
    npm --version
    ```

9. 你应该会看到已安装的 Node.js 和 npm 版本。

> [!IMPORTANT]
> Copilot app 中的智能体也必须能够运行 `node --version` 和 `npm --version`。在常用终端中成功运行，并不保证 app 也能访问这些命令。安装 Node.js 或更改 PATH 后，请完全退出并重新打开 app。如果某个会话中仍无法运行命令，请在继续之前按照[会话环境故障排除步骤][session-environment]操作。

> [!IMPORTANT]
> 每个工作树也需要项目依赖项和 Playwright Chromium 才能运行端到端 (E2E) 检查。准备工作树时，请遵循 Tailspin Toys 存储库 README 中的说明，并在批准任何安装请求前先审查请求内容。

## 设置实验存储库

你将在自己的 Tailspin Toys 项目副本中进行操作。现在从[模板存储库][template-repository]创建副本。新存储库包含实验所需的所有文件；安装 app 后，你会将它连接到 app。

1. 在新的浏览器窗口中，转到本实验的 GitHub 存储库：`https://github.com/github-samples/tailspin-toys`。
2. 在实验存储库页面上选择**使用此模板**按钮，然后选择**创建新存储库**，创建自己的存储库副本。

    ![“使用此模板”按钮，下拉列表中选中了“创建新存储库”](../../../_images/app-0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![“创建新存储库”表单，其中将 github-samples/tailspin-toys 设为模板并填写了存储库名称](../../../_images/app-0-create-repository.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），之后的实验步骤中会用到它。

> [!NOTE]
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题。整个研讨会中都会使用这些议题，无需自行创建。

请使用一份全新的研讨会模板副本。它包含存储库指令、应用程序代码、测试、quality-checks 技能和现有画布扩展。研讨会中你会自定义技能并创建 QA 智能体。如果使用旧副本，请向讲师确认其中包含所需文件。

## 总结和后续步骤

准备工作已完成！在本课中，你：

- 安装了 Node.js，以便在计算机上构建和测试项目。
- 使用模板创建了 Tailspin Toys 存储库的副本。

接下来，你将[安装 GitHub Copilot app][next-lesson]、连接刚创建的存储库并熟悉工作区。

## 资源

- [下载 Node.js][node-download]
- [从模板创建存储库][template-repository]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#从议题开始会话
