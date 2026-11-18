---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

本实验中，你让 GitHub Copilot 处理了多个改善用户体验和添加功能的任务，包括为代码添加文档、实现相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈，改进其工作。

## 审查工作

### 场景

如前所述，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能，然后再为团队启用代码审查。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会异步且自主地执行任务，因此必须遵守一些安全限制：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到提示，说明防火墙阻止了 GitHub Copilot。这是预期行为，因为默认情况下 Copilot 访问外部资源受到限制，包括调用外部 MCP 服务器。你可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看 PR 列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。留意 TSDoc 注释、TypeScript 约定和文件注释头；它们来自练习 2 中查看的自定义指令文件。
6. 确认更改合理后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 检查区域排队运行。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像对团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话处理修改。由于结果具有非确定性，这里无法提供适用于所有情况的固定提示。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求修改文档。可以使用上面的建议，也可以提出其他改进要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会显示新提交。
5. 选择 **Files changed** 选项卡检查更新。

可以继续迭代，直到满意为止。完成后，可将草稿 PR 标记为准备审查，再合并到 main 分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在检查 Copilot 为游戏详情页添加相关游戏功能的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改合理后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 检查排队的工作流，并确认单体 Astro 应用的项目检查通过。此过程可能需要几分钟。
9. **可选：**在 Codespace 中手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。你可以通过在 PR 的 **Conversation** 选项卡中添加评论，继续要求云智能体修改，例如更改显示的相关游戏数量，或补充文件注释头和 TSDoc 注释。请记住，此议题是在更新自定义指令之前分配的。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。这个 PR 应包括练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现时，重点留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - ARIA 标签、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改合理后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 确认排队的工作流检查通过。此过程可能需要几分钟。
10. **可选：**在 Codespace 中手动测试无障碍功能。检出 Copilot 创建的分支：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后仍保留设置。

注意自定义无障碍智能体如何引导 Copilot 遵循无障碍最佳实践。如果发现问题或有改进建议，可以像之前一样在评论中提及 **@copilot**。

## 可选练习——继续委派

云智能体适合处理真实待办事项，让你能同时专注于其他工作。试着为存储库创建一些议题并分配给 Copilot。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列课程中，你：

- **检查了存储库附带的自定义指令文件**，之后可以在云智能体输出中看到它们的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式开关。
- **使用 Copilot 智能体页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代云智能体创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

云智能体学习路径已完成。其他学习路径可以帮助你继续探索：

- 🖥️ **[VS Code 学习路径](../../vscode/)**——在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**——在终端完成两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

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
