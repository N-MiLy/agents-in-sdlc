---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，你就需要确保它能获取这些上下文。[指令文件][instruction-files]用于提供这些指导，让 Copilot 不仅了解你希望它做*什么*，还了解你希望它*如何*完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习流程不同，这里不会运行更新前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。之后在本学习流程中审查云智能体创建的拉取请求时，你会看到指令文件产生的影响。

## 指令文件

### 场景

作为一家优秀的软件开发公司，Tailspin Toys 为开发实践制定了一套指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 获得正确的信息，并按照上述实践执行任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以通过这种强大的功能引导 Copilot 提供更相关的建议和代码片段。例如，可以指定偏好的编码规范、库，甚至注释风格。你可以为整个存储库创建指令，也可以为特定文件类型创建任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。此文件应包含项目级信息，即与发送给 Copilot 的大多数聊天或 CLI 请求相关的上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 等特定语言，或为创建 UI 组件或一组新单元测试等任务提供指南。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一个编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

详细讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体来说：

- 让 `copilot-instructions.md` 专注于项目级指南，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指令。
- 使用自然语言，保持指南清晰，并提供代码应有和不应有的样例。

创建指令文件没有唯一方法，正如使用 AI 也没有唯一方式。通过尝试，你会找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应配备完善的指令文件。浏览此项目中的文件时，你可能会发现有适用于多种任务的指令，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同界面提供的方式有所不同（例如 VS Code 中的**配置聊天 → 生成智能体指令**，或 Copilot CLI 中的 `/init`）；当前界面的相关课程会在适用时介绍具体方法。
>
> 想查找模板或起点？请浏览包含大量指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件——其中包括一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定指令适用的文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都符合条件。
5. 留意此项目有关创建单元测试的专门指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可复用的文件，并为 Copilot 提供可参考的示例。（其中的路径相对于指令文件所在位置，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目的编码标准，但尚未要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑的内容：添加一项项目级规则，要求使用 TSDoc 文档注释和文件级注释头在代码中编写文档。之后在本学习流程中，云智能体处理分配给它的议题时会采用这项规则。

编辑前，请先创建一个分支（云智能体会读取议题目标分支中的指令，但仍建议将更改整洁地提交）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准添加文档要求，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。留意此处对项目编码标准的说明——但没有关于代码内文档的规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件 (`.github/copilot-instructions.md`) 中设置全局指南，也可以在 `*.instructions.md` 文件中为特定语言、文件类型或任务提供指令。存储库级文件适合放置刚添加的文档注释规则等项目范围的标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支——因此，指令更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。之后的练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支，直接提交到 `main`。这里保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从此项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项新的存储库级标准。之后在本学习流程中，云智能体创建的拉取请求会体现该标准。

接下来，你将[把第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源汇总][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
