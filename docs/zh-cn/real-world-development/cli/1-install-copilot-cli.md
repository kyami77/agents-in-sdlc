---
title: "练习 1 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉环境并找到预置的筛选议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一个功能强大的代理式编码助手，可在终端中运行，让开发者通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它可以帮助分担任务、请求变更，并保持专注。第一步自然是安装这个工具，好在可以使用已经熟悉的工具来完成。

在本练习中，将学习如何：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成验证。
- 信任工作坊存储库并尝试快速对话。
- 通过内置 GitHub MCP 服务器查找筛选议题。

## 场景

团队开始使用 AI agent 处理不断增长的积压工作。Copilot CLI 将这种能力带入终端，而终端本就是许多开发者的主要工作环境。本练习会完成安装和验证，为后续工作坊中的使用做好准备。

## 在 codespace 中打开终端

安装 Copilot CLI 前，需要先在 codespace 中打开终端窗口。

1. 如果当前不在 codespace 中，请先返回。
2. 按 <kbd>Ctrl</kbd>+<kbd>\`</kbd> 打开终端窗口。
3. 应该会在 VS Code 窗口底部看到终端面板。

## 安装 Copilot CLI

可以通过 [npm][install-npm]、[WinGet][install-winget] 和 [Homebrew][install-homebrew] 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本练习使用 npm 安装 Copilot CLI。

1. 在终端中确认 Node.js 已安装，并满足版本要求：

   ```bash
   node --version
   ```

   应看到版本 24 或更高。

2. 使用 npm 在 codespace 中全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

3. 通过查看版本验证安装：

   ```bash
   copilot --version
   ```

   应看到显示的版本号（例如 `v1.0.XX`）。

> [!TIP]
> 如果遇到权限错误，在某些系统中可能需要使用 `sudo npm install -g @github/copilot`。不过在 GitHub Codespaces 中通常不需要这样做。

## 使用 GitHub 完成验证

首次启动时，Copilot CLI 会提示使用 GitHub 账户完成验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果当前尚未登录，会看到验证提示。Copilot CLI 会显示一个设备代码，并要求访问一个 URL。
3. 按照屏幕上的说明操作：
   - 在浏览器中打开提供的 URL
   - 在提示时输入设备代码
   - 授权 Copilot CLI 访问 GitHub 账户
4. 验证完成后，会看到 Copilot CLI 提示符，可以开始输入问题和命令。

> [!NOTE]
> 在 codespace 中，可能已经通过 GitHub 会话完成验证。如果 Copilot CLI 启动时没有提示验证，就表示可以直接使用。

## 信任工作坊存储库

Copilot CLI 会显示以下提示：

```plaintext
Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

Do you trust the files in this folder?
```

确认显示的路径属于 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**。

## 熟悉 Copilot CLI

普通 shell 提示符中的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看当前安装版本中可用的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，注意 Copilot 会先探索存储库再作答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 使用以下提示让 Copilot 查找筛选议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读议题。完成快速首次更改后会继续使用它。

> [!TIP]
> 普通 Copilot CLI 会话会在终端当前检出的分支上工作，不会自动创建工作树。每次更改前都要创建功能分支。

## 使用工作坊快捷方式

Copilot CLI 通常会在使用尚未获得权限的工具前征求批准。此工作坊会在 Codespace 中使用 `--yolo` 重新启动 CLI，移除这些批准提示，让你专注于练习。

> [!CAUTION]
> `--yolo` 会启用完整自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或 VM 等隔离环境中使用，绝不要将其设为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

`--enable-all-github-mcp-tools` 会启用后续课程用于处理议题和拉取请求的 GitHub MCP 读写工具。Codespace 限制了对本地计算机的访问，但经身份验证的 GitHub 资源仍是真实资源。发布或合并前请审查更改。

1. 使用 `/exit` 退出 Copilot CLI。
2. 从存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再提出一个关于项目的简单问题，确认会话正常，然后使用 `/exit` 退出。

Copilot 会自动保存对话。之后更新指令或添加智能体时，会使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和后续步骤

恭喜，已成功安装并验证 GitHub Copilot CLI。现在已经学会如何：

- 使用 npm 安装 Copilot CLI。
- 使用 GitHub 账户完成验证。
- 信任工作坊存储库并尝试了快速对话。
- 通过内置 GitHub MCP 服务器找到了筛选议题。

接下来开始[第一项聚焦更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-copilot-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-copilot-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[install-npm]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-npm-all-platforms
[install-winget]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-winget-windows
[install-homebrew]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli#installing-with-homebrew-macos-and-linux
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
