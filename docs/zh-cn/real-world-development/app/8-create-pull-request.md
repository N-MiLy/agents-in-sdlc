---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能、指令更新、技能更新、质量保证 (QA) 配置和测试都已保存在同一个分支中。现在该一起审查它们并创建拉取请求了。你已自行合并星级评分的拉取请求 (PR)；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，我们会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为精简研讨会流程，你已在同一个会话和分支中完成完整的筛选和质量工作，并将所有更改放入这个 PR。

在本课中，你将：

- 了解 Agent Merge 及其如何自动处理合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 仅在审查完成后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时由开发者掌控合并授权。

## 认识 Agent Merge

**Agent Merge** 会在 GitHub Copilot app 中自动处理合并拉取请求所需的剩余工作。启用后，app 会话会读取你的拉取请求并处理阻碍合并的事项——修复失败的持续集成 (CI) 检查、回复审查意见，并在需要时变基——然后在 GitHub 允许时合并 PR。它会在后台运行，即使 app 重启后也会继续，并在拉取请求合并后自动关闭。

此前，你一直自行选择**合并拉取请求**。现在可以让 Agent Merge 负责这项工作，但要让它编辑代码并合并 PR，仍需你明确授权。授予合并权限前，请先审查它可以执行的操作及相关工作。

## 使用 Agent Merge 管理 PR

所有代码都已完成并审查后，可以让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁边的下拉列表。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅选择下拉选项并不会启动该流程。

Agent Merge 流程随即启动。它将：

- 创建标题和说明适当的拉取请求。
- 如果会话从议题开始，则在 PR 正文中引用相关议题。
- 变基，或处理与目标分支之间可能出现的合并冲突。
- 监控 CI 流程，确保所有检查均通过。
- 监控其他开发者或 Copilot 代码审查的 PR 反馈，并进行相应更新。
- 在所有检查通过后，选择是否自动合并 PR。

让 Agent Merge 在所有检查通过后也合并 PR！

5. 授予合并权限前，请审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁边的下拉列表。
7. 确认 **Merge pull request** 旁边已选中复选框。
8. 明确请智能体合并 PR：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进智能体进度，并解决报告的任何阻碍。继续之前，确认 PR 状态为 **Merged**；仅启用了合并权限或智能体仍在运行，并不代表 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护措施或缺失的权限。继续之前，请先解决这些阻碍。

## 总结和后续步骤

你已自动处理开发过程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查完成后授权 Agent Merge，并确认 PR 已合并。

接下来，你将[使用现有画布并创建分诊画布][next-lesson]，探索使用智能体检查、规划和可视化工作的更多方式。

## 资源

- [在 GitHub Copilot app 中管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
