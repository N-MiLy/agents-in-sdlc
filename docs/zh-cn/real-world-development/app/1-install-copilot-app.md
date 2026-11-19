---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接从模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款面向智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道都可以开箱即用。它专为同时引导多个智能体的工作流设计——每个智能体都有独立的工作区——帮助你减少亲自完成的重复性工作。安装 Node.js 并准备好项目副本后，下一步就是安装 app 并连接该存储库。

在本课中，你将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到 app。
- 熟悉工作区，包括模板为你预置的待办事项。
- 尝试一次快速聊天，了解 app 本身。

## 场景

团队正开始使用 AI 智能体处理不断增长的待办事项。Copilot app 为你提供了一个统一入口来引导这些工作——领取议题、运行智能体、审查更改并合并拉取请求。本课会指导你完成安装、连接项目，并熟悉如何开始有关项目的对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装和配置 GitHub Copilot app

要使用 GitHub Copilot app，首先需要安装它。Windows、macOS 和 Linux 均提供相应版本。现在安装 app、完成身份验证并将 Tailspin Toys 存储库添加到 app。

1. 在浏览器中打开 [GitHub Copilot app 主页][download-app]。
2. 下载适用于当前平台的 app，并按照主页上的说明安装。
3. 安装完成后打开 app。
4. 选择**登录 GitHub**，然后根据提示使用 GitHub.com 或 GHE.com 帐户登录。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **GitHub Enterprise Cloud**，并在提示时输入 `*.ghe.com` 主机名。

   如果使用 GitHub Enterprise Server，请先使用云帐户完成引导设置。然后打开 **Settings**，选择 **Accounts**、**Add account**，再选择 **GitHub Enterprise Server**。按照[企业服务器登录指南][ghes-sign-in]中的说明，在浏览器中授权 app。

5. 如果引导设置过程中出现存储库选择界面，请选择从模板创建的 Tailspin Toys 存储库。你也可以跳过存储库选择，在引导设置完成后再添加项目。
6. 按提示选择主题，然后完成引导设置。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需重复添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁边的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记下的所有者和存储库名称。
9. 确认存储库已显示在侧边栏中。在此处使用的布局中，`tailspin-toys` 等存储库文件夹会显示在 **Sessions** 下，每个存储库下方会列出单独的会话。

## 熟悉工作区

项目连接完成后，花些时间了解 app 的使用方式。app 会在侧边栏中组织几个区域：

- **New** - 可以在此处与 Copilot 开始新的聊天会话。
- **Pull requests** - 通过 app 的 GitHub 原生集成显示你的拉取请求。在此可以浏览和筛选拉取请求、查看 CI 状态并审查 PR，全程无需离开 app。
- **Issues** - 以相同方式显示你的议题。在此可以浏览和筛选议题，并从议题开始会话。
- **Automations** — 已保存的智能体任务，可按计划或按需运行。它们适合管理待办列表、定期项目维护或其他希望交由智能体处理的繁琐工作。总结部分会将它们作为后续探索方向，而非本研讨会的另一项练习。
- **Customize** - 以 MCP 服务器、插件、技能和其他组件的形式，为 Copilot app 添加功能。你将使用此区域配置 Playwright MCP。
- **Chats** — 轻量级对话，适合提问和头脑风暴，不需要自己的分支或工作区。开始第一次聊天后，此部分会显示出来。如果未显示，请选择 **Sessions** 旁边的 **+**，开始第一次聊天。
- **Sessions** — 按存储库文件夹分组显示智能体会话。选择 `tailspin-toys` 等存储库名称可打开其存储库视图；选择存储库下方的会话可返回该对话。每个会话都在独立的隔离工作区中运行，因此可以同时运行多个会话，而不会发生更改冲突。添加星级评分时，你将开始第一次会话。

随着研讨会逐步进行，你会进一步了解工作区！

> [!TIP]
> 遇到问题时，可以直接询问 Copilot！如果不确定如何操作，或不知道是否支持某项功能，可以向 Copilot 提问，它会帮助你找到方向。

### 查找预置的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在查看创建模板时预置的待办事项。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而非其下方的某个会话。然后在存储库的顶部导航中选择 **Issues**。
2. 根据标题查找以下议题，不要假设它们的编号固定：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题并阅读详情。每个议题也都可以作为启动智能体会话的入口。完成一项简短的首次更改后，你会从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的存储库中的事项。想查看其他存储库的工作事项？将那些存储库也添加到 app！

## 尝试快速聊天

熟悉 app 的一种好方法是使用它来了解 *app 本身*；**快速聊天**正适合此用途。快速聊天可以让你提问或进行头脑风暴，而不会创建分支或工作树，因此非常适合快速、一次性的问题，无需创建会话。

1. 在侧边栏中，选择 **Chats** 旁边的 **+** 打开新聊天。如果尚未显示 **Chats**，请选择 **Sessions** 旁边的 **+** 开始第一次聊天；之后 **Chats** 会显示出来。
2. 询问 app 的会话如何工作：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。你会看到每个会话都在独立的 Git 工作树中运行——这使你能够同时运行多个智能体，而不会发生更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜！你已安装 GitHub Copilot app、连接项目并熟悉工作区。你已了解如何：

- 安装 app 并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中查找预置的待办事项。
- 使用快速聊天提出简短的一次性问题。

接下来，你将[开始第一个智能体会话][next-lesson]，在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
