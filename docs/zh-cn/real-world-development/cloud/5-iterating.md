---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在整个实验过程中，你与 GitHub Copilot 一起完成了多个改善用户体验和添加功能的任务。你请 Copilot 为代码添加文档、为设计团队实现相关游戏功能以供迭代，并实现包含高对比度和浅色模式切换的无障碍功能。现在来查看代码更改；如有必要，请向 Copilot 提供反馈以改进成果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码并遵循正常的 DevOps 流程。因此，在让团队其他成员开始审查之前，先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全和 GitHub Copilot 云智能体

由于 Copilot 云智能体会在无人监督的情况下异步执行任务，因此我们设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只有存储库的读取权限，并且**只能**写入用于处理代码的分支。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境进行工作。
- 任何 GitHub Actions 工作流都必须经过人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。你将使用 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于**添加缺失文档**的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求将带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下，也可以回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

    浏览更改时，查找 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自你在练习 2 开始时审查过的自定义指令文件。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的状态指示器。
8. 如果有工作流正在等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流进入拉取请求检查部分的队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在拉取请求中提及团队其他成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理相关要求。由于结果具有非确定性，我们无法提供确切措辞，但可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中请求修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上面的任一示例，也可以提出其他文档方面的修改建议。
2. 选择**查看会话**，观察 Copilot 完成工作。留意 Copilot 如何启动新会话以进行更新。
3. 选择**返回拉取请求**，即可返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，你应该会看到 PR 中有新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿状态改为准备审查，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 创建的 PR，查看它如何解决在游戏详情页显示相关游戏的问题。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于**在游戏详情页显示相关游戏**的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准的状态指示器。
7. 如果有工作流正在等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
8. 你应该会看到工作流进入拉取请求检查部分的队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经实现相关游戏功能！与之前一样，你可以继续与 Copilot 云智能体迭代并请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或确认添加了注释头和 TSDoc 文档注释（请记住：分配此任务时，你还没有更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中添加新评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的无障碍功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于**为网站添加高对比度模式**的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求将带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 用于管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的状态指示器。
8. 如果有工作流正在等待批准，请选择**批准并运行工作流**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)
9. 你应该会看到工作流进入拉取请求检查部分的队列。如果一切正常，单个 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，在浏览器中测试高对比度和浅色模式切换功能，并确认页面重新加载后设置仍然有效。

留意无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理待办事项中的实际任务，让你可以专注于其他工作。为培养这种工作习惯，可以在存储库中创建更多议题并分配给 Copilot。以下是一些建议：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习流程。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，以便之后查看它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换功能。
- **使用 Copilot 智能体页面作为任务控制中心**，在无障碍会话执行过程中监控并引导它。
- **审查并迭代改进了云智能体的拉取请求**，使用 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习流程。若想继续探索，其他学习流程可以补充你在这里实践的内容：

- 🖥️ **[VS Code 学习流程](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习流程](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
