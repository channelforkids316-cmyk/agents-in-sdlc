---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI、完成身份验证、熟悉环境并找到已创建的筛选功能议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一个强大的代理式编码助手，可在终端中运行，让你通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它可以帮助你分担任务、请求更改并保持专注。第一步自然是安装这个工具，好在可以使用熟悉的工具来完成。

本课将完成以下内容：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 帐户完成身份验证。
- 信任工作坊存储库，并尝试快速对话。
- 通过内置 GitHub MCP 服务器找到筛选功能议题。

## 场景

团队正在采用 AI 智能体来处理不断增加的待办事项。Copilot CLI 将这种能力带入许多开发者日常使用的终端。本课将帮助你完成安装和身份验证，为后续研讨会做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 和 Homebrew 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本课使用 npm。

1. 返回 Codespace 并打开终端。
2. 验证 Node.js 已安装并满足版本要求：

   ```bash
   node --version
   ```

   应看到版本 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应显示版本号。

## 使用 GitHub 完成身份验证

首次启动时，Copilot CLI 会提示你使用 GitHub 帐户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果系统提示，请按照设备代码说明进行身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认路径指向 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**，表示信任该文件夹。

> [!NOTE]
> 在 Codespace 中，你可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有提示身份验证，就可以继续使用。

## 熟悉环境

在普通 shell 提示符下输入的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看已安装版本中的可用命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，留意 Copilot 会先探索存储库再回答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 让 Copilot 查找筛选功能议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开该 URL 并阅读议题。完成一项快速的首次更改后会继续使用它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都要先创建功能分支。

## 使用工作坊快捷方式

通常，Copilot CLI 在使用当前权限范围之外的工具前会先询问。为了让你专注于练习，本研讨会会使用 `--yolo` 重新启动 CLI。这个经用户批准的快捷方式会在 Codespace 中移除这些审批提示。

> [!CAUTION]
> `--yolo` 会启用所有自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只在 Codespace 或虚拟机等隔离环境中使用，切勿将它设置为日常开发的默认方式。详情请查看[允许和拒绝工具使用][allow-all-warning]。

在本研讨会中，`--enable-all-github-mcp-tools` 会启用后续课程用于处理议题和拉取请求的 GitHub MCP 读写工具。Codespace 限制了对本地计算机的访问，但通过身份验证的 GitHub 资源仍是真实资源。发布或合并前请审查更改。

1. 输入 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再询问一个关于项目的简单问题，确认对话可以正常工作，然后输入 `/exit` 退出。

Copilot 会自动保存对话。之后更改指令或添加智能体时，可以使用 `copilot --resume` 返回同一个功能会话和分支。

## 总结与后续步骤

本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 帐户完成了身份验证。
- 信任了工作坊存储库并尝试快速对话。
- 通过内置 GitHub MCP 服务器找到了筛选功能议题。

接下来，你将[开始第一项范围明确的更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
