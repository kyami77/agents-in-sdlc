---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种关键方式是使用[指令文件][instruction-files]指导代码生成。

本练习将：

- 探索如何通过存储库自定义指令和按路径限定范围的指令文件向 Copilot 提供项目上下文、编码指南和文档标准。
- 在现有指令下生成筛选功能的第一块数据切片（publishers 辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循你设置的某些标准。Copilot 具有非确定性。本练习的目标是观察更新指令后行为变化的*趋势*，而不是要求输出逐字符一致。

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

打开编辑器或 GitHub Web UI，阅读存储库中已有的核心 `copilot-instructions.md` 和面向不同任务的 `*.instructions.md` 文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看其中针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置一个相对于存储库根目录的 glob，用于确定适用文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看该文件中针对本项目单元测试的专门指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部的链接。它链接到其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件。这样能将较大的指令集拆分成更小、可复用的文件，并为 Copilot 提供示例。路径是相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中创建的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动新对话。模式和模型选择因练习而异；每一步会在需要时说明。

先在现有指令下发送提示，观察自定义指令的影响。稍后更新指令并重新发送相同提示。

1. 关闭之前练习遗留的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使没有明确要求，也可能添加文档注释。若出现这种情况也没关系；本练习要观察的是更新指令后的一致性提升。

## 添加新的存储库标准

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
> 可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指导，也可以创建 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 publisher 数据访问辅助函数。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 生成的函数现在应包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回包含 id 和 name 的所有 publisher 列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

现在已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会继续扩展这些代码。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要使用源代码管理。提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持性更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并将其关联到待办事项中的文档标准议题。GitHub 工具已连接到项目，可以让 Copilot 查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已经连接 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选择 **Agent**。
3. 要求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的 publishers 辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求你 **Allow** 其代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你 **Allow** 其代表你执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何使用*现有*指令生成代码。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot——指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
