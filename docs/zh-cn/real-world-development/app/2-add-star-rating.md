---
title: "第 2 课 - 添加星级评分：快速完成一项小改进"
description: "在 GitHub Copilot app 中开始第一个智能体会话，为游戏卡片进行小幅更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

熟悉工作区并尝试快速聊天后，现在可以开始一个**智能体会话**，对项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示评分。你将请智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

在本课中，你将：

- 开始智能体会话并了解会话结构。
- 请智能体对项目进行一项小而聚焦的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示评分。不过，主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让智能体在每张卡片上显示现有评分。这项小而独立的更改非常适合作为你的第一次会话练习。

## 会话的组成

**会话**是一个智能体在独立工作区中运行的对话。每个会话都有专用的 **Git 工作树和分支**，因此你可以同时运行多个会话（例如，一个添加功能，另一个修复 bug），而不会发生更改冲突。会话会按存储库分组显示在侧边栏中；选择任一会话即可切换到该会话。

会话中有三类内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及显示差异的**更改文件**列表。

## 开始会话并请求更改

开始一个新会话，探索项目并实现功能。在[设置 app][prior-lesson]时，你已从 GitHub 存储库添加项目。现在为该存储库创建新会话并请求更改。

1. 返回或打开 GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，开始会话。
3. 为存储库选择 `tailspin-toys`。
4. 在提示框下方选择**新工作树**和 **Interactive** 模式。使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，也就是项目的隔离副本。然后它会探索项目，找到添加新功能所需的文件，并创建相应代码。现在，你已使用 Copilot app 添加了一项新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小也不例外。现在就在 Copilot app 中查看这些更改。

1. 在 app 右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看更改过的文件和差异。

    ![GitHub Copilot app 顶部工具栏，其中箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 你应该会看到 `GameCard.astro` 中添加了代码。该文件用于显示游戏详情，新增内容应类似以下代码：有评分时显示评分，`starRating` 为 `null` 时显示“尚无评分”：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此你的具体代码可能有所不同，但总体上应该相似。

## 检查更改

打开浏览器前，请先审查智能体自动检查的结果。确认测试覆盖数字类型的 `starRating` 和 `null` 后备状态。缺少先决条件或跳过的检查不算通过；批准任何安装请求前，请先审查请求内容。

当然，我们不能只读代码就假定它能正常工作。让我们请 Copilot 打开网站，以便检查更新后的用户界面 (UI)。我们会请它启动网站，然后在浏览器画布中打开网站。

> [!TIP]
> 画布是可以直接在 Copilot app 中使用的交互式小组件。之后你会了解自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示，请 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 片刻之后，应用会启动，Copilot app 中会打开浏览器窗口。
3. 确认已评分的游戏卡片会显示满分为五分的评分。
4. 完成后，使用以下提示请 Copilot 停止它为此会话启动的开发服务器：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

你已完成这项功能！现在该创建拉取请求 (PR)，将新代码合并到现有代码库中了。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser** 并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联议题才能创建 PR，请确认应遵循该流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。你可以按需审查拉取请求。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并拉取请求！

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot app 发布第一个更改！具体来说，你：

- 开始了智能体会话并了解了会话结构。
- 指示智能体对游戏卡片进行一项小而聚焦的更改。
- 在工作区差异视图中审查了更改。
- 在本地运行应用，并在浏览器中确认了星级评分。
- 创建了 PR 1、审查检查结果并明确合并了 PR。

接下来，你将[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
