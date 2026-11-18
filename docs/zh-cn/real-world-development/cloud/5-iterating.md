---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多项任务，以改进用户体验并添加功能：为代码添加文档、实现设计团队可继续迭代的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在审查代码更改，必要时向 Copilot 提供反馈。

### 场景

正如前面多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来先审查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改，再邀请团队成员审查。

## GitHub Copilot 云智能体的安全性

由于 Copilot 云智能体异步且无人监督地执行任务，因此设有安全限制：

- Copilot 仅对仓库有读取权限，对用于生成代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个 PR：为代码添加文档。请使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> PR 中可能会出现 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为默认情况下 Copilot 对外部资源（包括外部 MCP 服务器）的访问受到限制。你也可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 在新浏览器选项卡中返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开标题类似 **Add missing documentation**（或更具体）的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成工作。这可能需要几分钟；可以先休息一下，或回顾已学内容。

3. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

4. 查看更新后的代码，包括新建的 TSDoc 文档注释和其他文档。具体更改可能不同；重点检查 TSDoc 注释、TypeScript 约定和文件注释头，它们来自练习 2 开始时审查的自定义说明。
5. 确认审查完成且更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 如果看到工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
7. 在 PR 的检查区域确认工作流已排队。若一切正常，单体 Astro 应用的项目检查应会通过。检查可能需要几分钟。

## 请求 GitHub Copilot 更改

你可以像在 PR 中评论其他团队成员那样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话来处理请求。结果具有不确定性，因此没有唯一的提示文本；可以要求 Copilot：

- 为每个代码文件顶部添加用途说明注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 在文档 PR 中添加评论并像提及其他用户一样提及 **@copilot**，提出文档更改请求。可以使用上面的建议，也可以提出其他希望添加到代码库的文档要求。
2. 选择 **View Session**，观察 Copilot 启动新会话并执行更新。
3. 选择 **Back to pull request** 返回 PR。
4. Copilot 完成更改后，PR 中会出现新提交。选择 **Files changed** 审查新更改。

可以不断迭代，直到满意为止。完成后，可将 PR 从草稿改为就绪状态并合并。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为游戏详情页显示相关游戏创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page**（或更具体）的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 如果看到工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. 在 PR 检查区域确认工作流已排队。若一切正常，单体 Astro 应用的项目检查应会通过。检查可能需要几分钟。
8. **可选：**在 Codespace 中手动测试功能。打开 Codespace 和终端，将 `<branch-name>` 替换为 Copilot 创建的分支名（例如 **copilot/fix-8**），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！你还可以在 **Conversation** 选项卡中发表评论，让云智能体迭代，例如调整显示数量，或要求添加注释头和 TSDoc 注释。注意：分配任务时尚未更新自定义说明。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心追加的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website**（或更具体）的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码，特别注意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - ARIA 标签、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript/TypeScript 代码。
5. 审查完成且更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 如果看到工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. 在 PR 检查区域确认工作流已排队，等待单体 Astro 应用的项目检查通过。
8. **可选：**在 Codespace 中手动测试无障碍功能。打开 Codespace 和终端，将 `<branch-name>` 替换为 Copilot 创建的分支名，然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   随后启动应用，在浏览器中测试高对比度和浅色模式切换，确认它们按预期工作并且页面重新加载后偏好仍然保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现问题，可以像处理其他 PR 一样在评论中提及 **@copilot**，要求它更新。

## 可选练习 — 继续委派任务

当云智能体处理真实待办事项时，你可以专注于其他工作。继续练习这种方式：在仓库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。本系列课程中，你：

- 检查了仓库自带的自定义说明，并观察其对云智能体输出的影响。
- 将议题分配给云智能体，并观察它设置环境、规划和异步执行。
- 创建并使用无障碍自定义智能体，添加高对比度和浅色模式切换。
- 使用 Copilot 智能体页面监控并引导无障碍会话。
- 审查并迭代云智能体创建的 PR，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体工作流已完成。其他工作流可以帮助你继续探索：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体连接。

也可以在自己的仓库中尝试以下内容：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

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
