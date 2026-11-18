---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在仓库 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专注的指令、最佳实践、代码模式和领域知识，帮助 Copilot 完成特定任务。团队可以把专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 合规审查的无障碍智能体、遵循安全编码实践的安全智能体，或维护测试模式一致性的测试智能体。

自定义智能体可在项目 `.github/agents` 中定义，也可全局定义在 `~/.copilot/agents`。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML 前置元数据，以及描述智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

两者都主要通过 Markdown 文件定义，并指导 AI 执行任务。简单来说，**自定义智能体是执行工作的角色，技能是它使用的工具**。自定义智能体有自己的上下文窗口，可协调技能甚至其他智能体。本实验中的无障碍智能体会根据无障碍指南审查和更新网站，并可调用 PR 工作流或测试技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法；应针对环境和场景测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 希望所有用户都能访问其众筹平台。用户反馈指出暗色主题的文字和背景对比度不足，因此设计团队要求添加可以切换的高对比度模式。本课将审查现有无障碍智能体，并使用它实现该功能。

## 检查无障碍自定义智能体

返回 codespace，从 `main` 创建新分支，以便无障碍工作与练习 3 的筛选 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`，查看包含 `name` 和 `description` 的 YAML 前置元数据。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` 前置元数据。

2. 阅读后续部分，了解无障碍网站代码的核心职责、最佳实践、HTML/CSS/JavaScript 示例和常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择智能体即可让会话使用它的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 返回 codespace，打开 Copilot Chat 并选择 **New Chat**。各练习所需模式和模型可能不同，步骤中会注明。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。

   ![Chat 视图中显示智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉列表（默认显示 **default**），再选择 **Accessibility agent**。
3. 发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会以内联差异的形式显示。
5. 查看 **Files changed** 指示器，确认智能体更改的文件。

> [!NOTE]
> 过程可能需要几分钟。Copilot 会编辑 Astro 组件、CSS 和相关测试等文件。下一练习中将审查并引导这项工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义智能体][custom-agents]，以及如何在 Copilot Chat 智能体模式中使用无障碍智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查更改并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
