---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你通常需要找到所有相关文件、进行更改、运行测试、调试、找出漏掉的文件、再进行修改……事情往往没完没了。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈并与用户交互。云智能体则像团队中的结对程序员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习中你已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先探索 Tailspin Toys 网站，了解它当前的功能。

此网站是一个带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/` 中；交互功能则通过使用标准 DOM API 的小型 Astro `<script>` 添加。

### 启动网站

为了更轻松地运行网站，项目提供了一个 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，可以在 GitHub Codespace 中运行该任务：

1. 返回 Codespace。上一练习结束后，你应该已切换回 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，你应该会看到类似以下内容的横幅，其中显示访问 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭它。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有通道，现在你的网站服务器就托管在其中！

### 探索网站

网站启动后，可以探索它的功能。主要功能包括：

- **主页：**显示棋盘游戏列表及其标题、图片和描述。
- **游戏详情页：**选择游戏后会打开详情页，其中包含更多信息，例如标题、描述、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；各步骤会在相关处说明。

网站的初始实现已经可以正常工作，但我们希望添加新功能来改进它。先查看待办事项。使用模板设置项目时，系统已自动创建待办议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并不偏好某个特定模型。构建本研讨会时，我们使用了 Claude Sonnet 4.5，因此将它列入说明，希望你能获得相对一致的代码建议和良好的体验。不过，LLM 的输出具有概率性，因此你看到的建议可能与研讨会说明不同。这完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用其他 MCP 命令，但仍应该能够完成任务。

3. 向 Copilot 发送以下提示，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择**继续**，运行命令并列出所有议题。
5. 查看生成的议题列表。

留意 Copilot 还会根据它认为对用户最有用的事项为你排列优先级。

## 审查指令文件

让智能体开始生成代码前，正好可以审查为 Copilot 提供工作上下文的指令文件。你将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件了解如何为网站添加功能。

1. 在 Codespace 中转到 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各种文件类型专用指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于当前指令文件的位置。这使得指令可以重复使用，也可以将复杂内容拆分为较小、易于管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用程序进行三项独立更新：

- 添加或完善数据层 (`src/lib/`) 中剩余的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 智能体模式可以为你完成这些任务！现在来添加相关功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择**添加上下文**、**指令**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到与目标功能有关的文件。你应该能看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多项任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，这可能需要几分钟。在此过程中，代码编辑器中可能会出现**保留**和**撤销**按钮。Copilot 完成后，每项更改都会提供**保留**或**撤销**选项，因此不需要在工作进行时选择。

7. Copilot 提示时，选择**继续**以运行测试。

   ![Copilot Chat 面板中的对话框，询问用户是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能有测试失败。这没有关系！Copilot 会在代码生成和测试之间不断迭代，直到完成任务且不再发现错误。

   ![已完成的 Copilot 智能体模式聊天会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择**保留**。

## 总结和后续步骤

恭喜！在本练习中，我们了解了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。我们学习了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来，让我们[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增加，需要启用分页。使用本练习中学到的技能，请 Copilot 更新网站以实现分页。编写代码时可以考虑以下事项：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 考虑如何实现分页：允许用户选择每页大小，还是固定为某个值。
- 编写提示时，确保为 Copilot 提供所需指导，以便按你的预期实现功能。
- 你可能需要与 GitHub Copilot 迭代，通过请求更改并补充上下文来完善结果。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
