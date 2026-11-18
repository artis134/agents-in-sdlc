---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 中的自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在仓库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专注的指令、最佳实践、代码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 合规审查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML 前置元数据，以及用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行工作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是它使用的工具**。

自定义智能体有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要针对实际环境和场景测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将了解：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，当前暗色主题的文字和背景对比度不足，导致部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍访问至关重要，因此需要尽快实现。你将使用自定义智能体生成此功能。

## 检查无障碍自定义智能体

已为你创建了无障碍自定义智能体。返回 codespace 并查看其配置：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` 前置元数据。

3. 查看后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。可以将任务分配给 Copilot 云智能体、监控任务、重新引导任务并提供额外指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开仓库，选择 **Issues**，然后选择 **New issue** 和 **Blank issue**。
2. 将 **Title** 设为 `Add high contrast mode to website`。
3. 将 **Description** 设为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. 选择 **Create** 创建议题。
5. 在右侧选择 **Assign to Copilot**，再从自定义智能体列表中选择 **Accessibility agent**，然后选择 **Assign**。

   ![云智能体分配截图，其中突出显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 [GitHub Copilot 中的自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义智能体可以将团队专业知识和标准整理成可复用配置，指导 Copilot 更有效地完成工作。

本课中，你：

- 了解了如何定义自定义智能体。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，从任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中准备使用自定义智能体][org-custom-agents]
- [在企业中准备使用自定义智能体][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
