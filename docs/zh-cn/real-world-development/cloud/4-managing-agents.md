---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，你可以查看所有有未关闭拉取请求 (PR) 的智能体任务，检查已执行的操作，还可以引导正在进行的会话。

本课将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

让智能体创建高对比度模式后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此顺便添加浅色模式很合适。现在引导智能体，让它同时实现高对比度和浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表，其中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和处理方案而异。

4. 同时查看右侧的拉取请求窗格，其中显示 PR 和已更改文件，便于进一步监控。

## 引导云智能体

现在在任务运行期间，要求 Copilot 在实现高对比度模式时一并添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题会根据 Copilot 使用的名称和当前工作状态而异。

   ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始处理任务。开始看到类似消息时，说明任务已进入此阶段。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面上的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 观察 Copilot 如何确认提示并将其纳入后续工作。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会在完成你发送消息时正在执行的步骤后，将新请求加入工作流程。

这可能需要几分钟，可以利用这段时间回顾已学内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有活动和已完成的任务、审查正在进行的工作，并重定向会话以调整范围或补充指导。

本课探索了：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在运行的会话，要求添加功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
