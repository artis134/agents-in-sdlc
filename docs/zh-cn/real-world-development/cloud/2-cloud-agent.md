---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以委派繁琐任务、加快开发流程，让开发人员专注于更重要的工作。

本课将了解如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初受雇开发网站的承包商没有充分编写文档；目前应用中的导出函数都缺少文档注释。团队首先希望为所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能：在每款游戏的详情页显示同一类别中的其他游戏，方便用户继续浏览。现在他们只需要能用于用户体验验收测试的初步实现。目前此功能尚未完成，但优先级低于其他议题。

这些都是容易被推迟的任务，适合分配给云智能体异步处理。开发人员可以继续处理其他工作，之后再审查结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进展、设置环境并开始工作。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，它会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法工具，使用它并不能省去思考过程。即使看似简单的操作也可能包含复杂细节，因此[应谨慎界定云智能体的任务范围][cloud-agent-best-practices]。像使用 AI 结对程序员一样，分阶段工作、学习、试验并调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本仓库中的自定义说明

此前的练习介绍了自定义说明及其作用。如果已完成相关练习，应该见过 `.github/copilot-instructions.md`；否则现在可以查看。分配任务前，以只读方式检查仓库中的说明文件，之后便能观察其影响：

- `.github/copilot-instructions.md`：检查 **Code standards**，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` 前置元数据，了解这些说明如何限定到 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，注意查看 PR 中的 TSDoc 注释、TypeScript 约定和文件注释头。稍后的练习还会再次审查这些更改。

## 设置云智能体的开发环境

创建代码通常需要配置特定环境并运行设置脚本；将任务分配给 Copilot 时也一样。云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建位于 `.github/workflows/copilot-setup-steps.yml` 的[专用设置工作流][setup-workflow]，在 Copilot 开始工作前安装必要的工具和依赖项。

实验已预先配置此工作流，用于安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用所需的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在仓库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤仅授予所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆仓库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆仓库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业，会在 Copilot 开始处理 PR 前通过 GitHub Actions 执行。
- `workflow_dispatch` 触发器允许你在仓库的 **Actions** 选项卡中手动运行工作流，以验证其是否成功，而不用等待 Copilot 启动。

## 添加文档

许多项目的文档过时或缺失，这类技术债务会降低效率，并增加维护代码库或引入新开发人员的难度。Copilot 很擅长生成文档，因此这是分配给云智能体的理想任务。稍后的练习会审查其工作。

1. 在新浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框，然后选择 **Blank issue**。
4. 将 **Title** 设为 `Code lacks documentation`。
5. 将 **Description** 设为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

8. 选择 **Assign**。
9. 选择 **Pull Requests** 选项卡。
10. 打开新创建的拉取请求（PR），标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现，请稍等片刻并刷新页面。
11. 几分钟后，Copilot 应会创建待办列表。

> [!CAUTION]
> Copilot 的待办列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。如果看不到更新后的状态指示器，可能需要刷新页面。

12. 检查待办列表和 Copilot 准备执行的任务。
13. 向下滚动 PR 时间线，确认 Copilot 已开始处理议题。
14. 选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

15. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并制定计划，以及编写代码。

整个过程可能需要几分钟。云智能体的目标之一是异步执行任务，让你能专注于其他工作。现在利用这一特点，再为云智能体分配另一项任务，然后继续为应用添加功能。

## 在游戏详情页添加相关游戏

云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。现在将设计团队的相关游戏功能分配给云智能体。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框，然后选择 **Blank issue** 模板。
4. 将 **Title** 设为 `Show related games on the game details page`。
5. 将 **Description** 设为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

8. 选择 **Assign**。

议题的第一条评论旁很快会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在继续创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体异步处理，解决技术债务、创建新功能，或协助将代码迁移到其他框架。

本课中，你：

- 自定义了代码生成环境。
- 了解如何确保操作安全。
- 了解了明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，你可以转而创建并使用自定义智能体。[云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前练习过的自定义说明。

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
