---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]可让你创建专门针对开发工作流中特定任务或领域的 AI 助手。通过在存储库的 `.github/agents` 文件夹中定义 Markdown 文件，你可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的智能体——例如，确保符合 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少指定 `name` 和 `description`，后面则是定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]有些相似之处。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在工作过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方法。与 AI 相关的其他内容一样，请进行测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前暗色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此你希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

在本练习中，你将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

我们已为你创建了一个无障碍自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建一个专用于无障碍工作的分支（这样可以将练习 3 中的筛选 PR 分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接着浏览并审查以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 下定义的所有自定义智能体。你可以选择自定义智能体，让聊天会话使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的新对话。模式和模型选择因练习而异；各步骤会在相关处说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（其中显示当前智能体，默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作——探索代码库、提出更改建议并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可供审查的差异。
6. 智能体完成编辑后，**Files changed** 指示器会更新，让你查看智能体修改的工作文件。

> [!NOTE]
> 这可能需要几分钟。Copilot 正在实际修改存储库——它会根据需要编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习中，你会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域的专业 AI 助手。使用自定义智能体，可以将团队专业知识和标准整理成可复用的智能体，更有效地引导 Copilot 完成特定类型的工作。

你了解了以下概念：

- 如何定义自定义智能体。
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并引导智能体工作][next-lesson]——在更改过程中进行审查，并在同一会话中添加浅色模式切换功能。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
