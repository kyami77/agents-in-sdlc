---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件，再次修改……这样的工作会不断重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可在 IDE 中更自主地工作。它会先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，适合快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习：

- Copilot 智能体模式如何探索项目、找到相关文件并协调多文件更改。
- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此你希望允许用户按类别和发行商筛选。上一练习已添加 publishers 辅助函数，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改前先探索 Tailspin Toys 网站，了解它当前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详情。它是一个单独的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/`，交互功能则使用少量范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 更新并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是一个私有隧道，网站现由你的 Codespace 托管。

### 浏览网站

网站运行后，可以探索以下主要功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后可查看其详细信息，包括标题、描述、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动新对话。模式和模型选择因练习而异；每一步会在需要时说明。

网站初始版本已可正常运行，但我们希望添加新功能。先查看待办事项。使用模板设置项目时，系统已自动创建议题列表。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此说明中包含该模型，希望建议结果相对一致。LLM 具有非确定性，你看到的建议可能与工作坊不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 具有非确定性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 查看指令文件

启动智能体生成代码前，先查看用于向 Copilot 提供上下文的指令文件。项目中的 [用户界面 (UI) 指令文件][ui-instructions]说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能时的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对不同文件类型的指令文件链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分复杂指令，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页，添加筛选 UI。

合并前还需要运行并通过测试。Copilot 智能体模式可代为完成这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择 **Agent**。
4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**。完成后，每项更改都会提供 **Keep** 或 **Undo** 选项；工作进行期间无需选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 可能会暂停，测试也可能暂时失败。这是正常的；Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能。
11. 确认功能正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、查找相关文件并协调更改。
- 合并前如何审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 延伸练习——实现分页

随着游戏列表增长，网站需要分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页实现方式，例如是否允许用户选择页大小，还是使用固定值。
- 编写提示时提供必要指导，以便 Copilot 按预期实现。
- 可能需要与 GitHub Copilot 多次迭代，提出更改并补充上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：用途及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
