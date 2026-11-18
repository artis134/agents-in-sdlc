---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项不同任务，以改进用户体验并添加功能。云智能体虽然可以异步自主运行，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有创建了未关闭 PR 的智能体任务，查看它执行的操作，并为正在进行的会话提供指导。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，请求添加功能。

## 场景

分配高对比度模式任务后，团队意识到也可以添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此将此功能一并纳入很合理。现在需要引导智能体同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表，其中应包括更新代码文档、生成游戏管理 API，以及为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的步骤，例如检出代码、创建工作环境、设置 MCP 服务器，以及完成任务的其他操作。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方案而变化。

4. 查看右侧显示的 PR 面板，以监控 PR 和更改的文件。

## 引导云智能体

现在请求 Copilot 在实现高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。确切标题可能会根据 Copilot 使用的名称和当前工作状态而异。
2. 观察会话几分钟，直到设置完成并开始执行任务。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

Copilot 会像之前一样处理更新后的任务！完成当前步骤后，它会将新请求加入工作流程。

此过程可能需要几分钟，正好可以暂停并回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有进行中和已完成的任务、审查工作，也可以重新引导任务并调整范围或补充信息。

本课中，你：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重新引导正在进行的会话，请求添加功能。

Copilot 正在完成无障碍功能，接下来[迭代改进它创建的拉取请求][next-lesson]。

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
