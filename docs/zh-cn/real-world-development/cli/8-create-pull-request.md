---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都已保存在同一分支上。现在一起审查这些更改并创建拉取请求。你之前手动合并了星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常应将功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本工作坊，你将筛选功能和质量工作流保存在同一对话和分支中，并通过一个 PR 一并提交。

本课将学习如何：

- 了解 Agent Merge 以及它如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能开发流程中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发人员掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 的阻塞事项——例如修复失败的持续集成（CI）检查、回应审查意见，以及在需要时变基——然后在仓库允许的情况下启用 GitHub 自动合并。

此前，你是自行选择 **Merge pull request**。Agent Merge 可以代为完成这项工作。启用 Agent Merge 前，请先审查更改并确认其已准备好合并。

## 使用 Agent Merge 管理 PR

所有代码都已创建，现在一起审查更改、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会跟进 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认收到类似 “PR #14 was squash-merged successfully.” 的 Copilot 消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、仓库设置或权限限制。如果流程受阻，请阅读报告的原因，并在仓库允许的情况下手动完成已审查的合并。

## 总结和后续步骤

你已自动处理开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。你：

- 了解了 Agent Merge 以及它如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享以及可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
