---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在整个实验中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建相关游戏功能以便迭代，并实现了包括高对比度和浅色模式切换在内的无障碍功能。现在一起检查代码更改；如有需要，也可以向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用代码审查前，先审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能所做的建议。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会在没有监督的情况下异步执行任务，因此它受到一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对用于提交代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流在运行前都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体生成的第一个拉取请求 (PR)，其中包含为代码添加的文档。通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，标题也可能更完整。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改会因 Copilot 的工作而异。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该能看到有些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 你应该能在拉取请求的检查区域中看到工作流已加入队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像 @ 提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供确切的请求文本。以下是一些可以要求 Copilot 更新的内容：

- 为每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求 Copilot 更改生成的文档；像提及用户一样使用 **@copilot**。你可以采用上面的建议，也可以提出其他希望在代码库中看到的文档更改。
2. 选择 **View Session**，观察 Copilot 执行工作。留意 Copilot 如何启动新会话来完成更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

你可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为准备审查，然后合并到 main 分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 创建的 PR，查看它对游戏详情页显示相关游戏议题的处理结果。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，标题也可能更完整。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该能看到有些工作流正在等待批准。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该能在拉取请求的检查区域中看到工作流已加入队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能。和之前一样，你可以通过迭代让 Copilot 继续更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（记住，在分配这项任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。该 PR 应同时包含你在练习 3 中分配的高对比度模式，以及在练习 4 的控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，标题也可能更完整。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现，尤其留意以下内容：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储功能。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该能看到有些工作流正在等待批准。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该能在拉取请求的检查区域中看到工作流已加入队列。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认切换状态能在重新加载页面后保留。

留意无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或可改进之处，可以像之前一样在评论中提及 **@copilot**，请求更新。

## 可选练习 — 继续委派工作

云智能体适合处理实际待办事项，让你可以把任务交出去并转而处理其他工作。可以在自己的存储库中创建几个议题并分配给 Copilot，养成这种协作方式。可以考虑以下内容：

- 在游戏详情页添加支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列练习中，你：

- **检查了此存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**来处理无障碍问题，添加高对比度和浅色模式切换。
- **将 Copilot 智能体页面用作控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成云智能体学习路径。如果要继续探索，可以学习其他路径，巩固本次练习的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中直接使用 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 从终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

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
