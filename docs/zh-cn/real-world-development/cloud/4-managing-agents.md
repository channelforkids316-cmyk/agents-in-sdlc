---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项任务，目标都是改善用户体验和添加功能。云智能体能够异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心中，你可以查看所有创建了未关闭拉取请求 (PR) 的智能体任务，了解它们执行的操作，甚至可以引导仍在进行的会话。

本课将完成以下内容：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，将这项功能一起实现也很合理。现在需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的工作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和执行方式而异。

4. 还要留意右侧出现的拉取请求 (PR) 面板。你可以在这里查看 PR 及其更改的文件，以便进一步监控。

## 引导云智能体

现在你已看到当前任务状态，可以要求 Copilot 在实现高对比度模式时加入浅色模式切换功能。

1. 选择与添加高对比度模式有关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始执行任务。出现与以下类似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认提示并将它纳入工作流程。

## 等待 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务。收到请求时，它可能正在处理某个步骤；完成该步骤后，它会将新请求加入工作流程。

和之前一样，这需要几分钟。你可以借此机会回顾目前学到和体验的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。在这里可以跟踪所有进行中和已完成的任务、审查智能体执行的工作，并重定向正在运行的任务以调整范围或提供额外指导。

本课探索了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
