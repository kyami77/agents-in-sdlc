---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的问题、需要更新的旧代码，或因缺少时间而积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，开发人员则可以专注于更重要的工作。

本课将探索：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有为项目编写文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示同类别的其他游戏，供用户继续浏览。现在无需精细打磨，只需实现可用于 UX 验收测试的功能。虽然这项工作目前受阻，但积压中还有优先级更高的议题。

这些都是容易被推迟的任务，适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他工作，之后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以在后台执行任务，就像人类开发人员一样。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置工作环境并开始处理任务。你可以在任务进行中或完成后查看 Copilot 会话。提议的解决方案准备好审查时，Copilot 会在拉取请求中通知你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，不会让你跳过思考。即使看似简单的操作也可能包含复杂细节，因此要注意为 Copilot 云智能体[明确界定任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并随时调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成相关练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中已有的指令文件，稍后便能观察它们的影响。

在 GitHub Web UI 或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些指令如何限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，注意观察它创建的拉取请求是否包含 TSDoc 注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件。

## 为 Copilot 云智能体设置开发环境

代码任务通常需要专门环境和设置脚本。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions] 环境。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在任务开始前运行，以便安装所需工具和依赖项。实验已预先配置此工作流，其中包括 Node.js、项目依赖项、端到端测试所需浏览器，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许在存储库的 Actions 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅授予设置步骤所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），需要 contents: read 权限。如果不在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它与其他 GitHub 工作流类似，但有几点值得注意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试设置流程，无需等待 Copilot 调用。

## 添加文档

每个人都知道文档很重要，但许多项目的文档过时或缺失。这类技术债务经常被搁置，降低效率，也增加维护和新成员上手的难度。Copilot 擅长生成文档，因此很适合将这项任务交给云智能体。之后的练习会审查它生成的成果。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 选择 **Assign**。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 创建待办事项列表可能需要几分钟。它需要先设置环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看 Copilot 将要执行的任务列表。
14. 向下浏览 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、确定方案并编写代码。

任务可能需要几分钟。云智能体的主要优势之一是能够异步处理任务，让你腾出时间做其他事情。接下来利用这一点，再分配一个任务给云智能体，然后自己为应用编写功能代码。

## 在游戏详情页添加相关游戏部分

使用 GitHub Copilot 云智能体的一大优点是可以拆分工作：你专注于一组任务，Copilot 则处理另一组。为设计团队添加相关游戏部分也许不需要很长时间，但这段时间仍可用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**。
4. 选择空白议题模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 选择 **Assign**。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

随后，议题的第一条评论中会显示 👀，表示 Copilot 已开始处理。

![Copilot 用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需一直监控，可以等任务完成后再审查结果。现在继续了解自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、创建新功能，或协助迁移代码框架。

本课探索了：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何将议题分配给 Copilot。

Copilot 云智能体会在后台继续工作。现在可以创建并使用自定义智能体。[云智能体还支持使用 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
