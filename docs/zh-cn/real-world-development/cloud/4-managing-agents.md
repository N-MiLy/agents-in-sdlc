---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你要求 Copilot 云智能体分别处理三个任务，以改善用户体验并添加功能。云智能体虽然设计为自主异步运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。你可以在这个任务控制中心查看所有包含未关闭拉取请求 (PR) 的智能体任务、查看它们执行的操作，甚至引导正在进行的会话。

在本课中，你将：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到现在也适合添加浅色模式。既然正在更新网站样式并添加切换功能，那么一起实现浅色模式很合理。你将引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

现在查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择其中一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行各种步骤以完成分配的任务。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前的工作状态和所采用的方法而异。

4. 还要留意右侧出现的拉取请求 (PR) 窗格。在此可以查看 PR 和更改过的文件，以便进一步监控。

## 引导云智能体

现在你已查看正在进行的任务，可以请 Copilot 在实现高对比度模式的同时添加浅色模式切换功能。

1. 选择与添加高对比度模式有关的会话。确切的标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到会话显示已完成设置并开始工作。出现类似以下消息时，表示它已开始工作。
3. 在**Copilot 正在工作时引导活动会话**对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面上的云智能体任务，其中突出显示了“Copilot 正在工作时引导活动会话”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后将新请求加入工作流程。

与之前一样，这需要几分钟，正好可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以通过此任务控制中心跟踪所有活动和已完成的任务、审查正在执行的工作，甚至重定向正在处理的任务以调整范围或提供额外指导。

你了解了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，现在你可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
