---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解你想要*什么*，也知道应当*如何*完成。

本练习将：

- 探索如何通过存储库自定义指令和按路径限定范围的指令文件向 Copilot 提供项目上下文、编码指南和文档标准。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会运行修改前后对比提示。Copilot 云智能体通过 GitHub 议题异步工作，因此难以实时并排展示指令效果。稍后的练习中，你可以在审查云智能体创建的拉取请求时看到这些指令的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件顶部都应有注释块，说明文件用途。

使用指令文件可确保 Copilot 获取正确信息，按这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。可以为整个存储库或特定文件类型创建指令。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此单一指令文件。它应包含项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时都会使用存储库级指令和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

深入讨论指令文件的创建方法超出了本工作坊范围，但示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的指导聚焦于项目级信息，例如项目结构、构建内容和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

指令文件没有唯一正确的写法，就像使用 AI 没有唯一正确的方法一样。通过实验找到最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件集合。查看本项目时，你会发现其中有适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的文件。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应学习路径会在相关课程中说明。
>
> 可从 [awesome-copilot][awesome-copilot] 查找模板和起点，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义指令文件

阅读存储库附带的指令文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看其中针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置一个相对于存储库根目录的 glob，用于确定适用文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看该文件中针对本项目单元测试的专门指导。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并查看底部的链接。它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样能将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供代码示例。相关路径是相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来只需编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体之后处理分配给它的议题时会使用这项规则。

先创建分支：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分已有编码标准，但尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在存储库级文件（`.github/copilot-instructions.md`）中编写全局指导，也可创建 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习中，Copilot 会从 `main` 分支开始处理分配的议题，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后云智能体处理议题时便会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上操作，可以跳过分支步骤并直接提交。本练习使用分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。之后的练习中，你会看到云智能体创建的拉取请求如何遵循该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
