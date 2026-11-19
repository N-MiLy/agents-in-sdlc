---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它在浏览器中确实有效。你无需亲自点击浏览应用，而是连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器为你测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以访问编辑器之外的资源，这会大幅增强它的能力。Copilot 通过 Model Context Protocol (MCP) 实现这一点——MCP 是一种让智能体与外部工具和服务通信的标准方式。

![智能体模式的内部工作原理及其如何与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。使用 MCP，AI 智能体可以实时与外部工具和服务通信，通过资源获取最新信息，并使用工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理两者之间的通信，例如现有 API 或 npm 包等本地工具。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

以下是几个常用的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)：**提供一组用于管理 GitHub 存储库的 API。AI 智能体可以借此创建新存储库、更新已有存储库，并管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)：**使用 Playwright 提供浏览器自动化功能。AI 智能体可以借此浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可以提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，有助于发现并参与生态系统。

> [!CAUTION]
> 从安全角度来看，对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前请仔细审查其源代码、验证发布者，并考虑安全影响。只使用你信任的 MCP 服务器；授予其访问敏感资源或执行敏感操作的权限时应谨慎。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含这个 `github` 条目，因此 Copilot 才能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，所以无需在本地安装任何内容——VS Code 通过 HTTP 连接到它；Copilot 首次使用其工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中的 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是一个**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在不显示窗口的情况下运行浏览器；Codespace 中没有可显示浏览器的桌面，因此必须使用此设置。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 之后报告浏览器缺失，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择该操作立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便服务器启动。

手动启动是可选的。如果跳过这一步，VS Code 会在下一步骤中 Copilot 首次需要这些工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、浏览、应用筛选条件并读取结果，然后向你总结观察结果，因此无需离开当前对话，也不必亲自点击应用，就能快速确认功能是否符合预期。

在底层，Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是屏幕截图。也就是说，智能体会像辅助技术一样，根据带有标签的结构化元素（按钮、链接、列表项）进行推理，因此快速的功能测试也能顺便检查基本的无障碍情况。

连接服务器并运行应用后，请 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。请根据议题中的验收标准审查其总结——如果有问题，可以提出后续问题，或让它修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告浏览器缺失，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示的更改](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不妨让它来创建 PR！它可以找到相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 请 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择**继续**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求你通过 GitHub 身份验证——按照提示授权即可。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但请**暂时不要**合并。

## 总结和后续步骤

恭喜！在本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建了拉取请求。

你已使用 MCP 服务器*测试*功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，让我们[创建自定义智能体][next-lesson]，以便更高效地完成无障碍审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
