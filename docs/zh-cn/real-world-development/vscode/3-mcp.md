---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你已使用智能体模式构建筛选功能。创建拉取请求前，应在浏览器中确认功能正常。现在连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习模型上下文协议（MCP）如何通过新工具扩展 Copilot、如何在工作区添加 Playwright MCP 服务器，以及如何请 Copilot 使用浏览器手动测试筛选功能。

## 什么是模型上下文协议（MCP）？

智能体模式可以通过 MCP 访问编辑器之外的功能。MCP 是智能体与外部工具和服务通信的标准方式，支持实时访问信息并代表你执行操作。MCP 服务器充当 AI 智能体与外部工具（例如现有 API 或本地 npm 包）之间的桥梁，每个服务器提供一组工具和资源。

![智能体模式的工作原理示意图，展示其与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）的交互](../../../_images/ex1-mcp-diagram.png)

常见服务器包括 [GitHub MCP Server](https://github.com/github/github-mcp-server)（管理 GitHub 仓库、议题和拉取请求）以及 [Playwright MCP Server](https://github.com/microsoft/playwright-mcp)（驱动浏览器、导航页面、填写表单和选择按钮）。GitHub 还托管了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前请审查源代码、验证发布者并评估安全影响。只使用信任的服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 配置当前 VS Code 工作区可用的 MCP 服务器。打开此文件，确认模板已配置 `github` 服务器：

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    }
  }
}
```

上一练习中 Copilot 能读取待办事项，就是因为项目模板提供了此配置。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时让你进行 GitHub 身份验证。

## 添加 Playwright MCP 服务器

Playwright MCP 服务器可为 Copilot 提供可控浏览器，用于测试功能。在 `.vscode/mcp.json` 中加入 `playwright` 条目：

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/"
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    }
  }
}
```

保存文件。与远程 `github` 服务器不同，`playwright` 是本地服务器，VS Code 通过运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数让浏览器在无窗口模式下运行，适用于没有桌面环境的 codespace。

> [!NOTE]
> Tailspin Toys 已使用 Playwright 运行端到端测试，通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 的 `playwright` 条目上方选择 **Start**，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任。

手动启动是可选的；若跳过，VS Code 会在下一步 Copilot 首次需要工具时启动服务器并询问是否信任。

## 测试筛选功能

Playwright MCP 服务器可以驱动真实浏览器。Copilot 会打开页面、应用筛选、读取结果并总结观察到的行为。它通过页面的[无障碍树][playwright-mcp-server]而不是截图工作，因此会根据有标签的按钮、链接和列表等结构化元素进行推理，也可以顺便检查基本无障碍行为。

确保服务器已连接且应用正在运行，然后发送以下提示：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器、执行测试并汇报结果。根据议题中的验收标准审查报告；发现问题时，继续提问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试要求应用运行于 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。首次使用 Playwright MCP 时，Copilot 可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支：

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 的更改。
2. 选择 **+** 暂存更改，然后使用 **Sparkle** 按钮生成提交消息。
3. 选择 **Publish** 将分支推送到仓库。

![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面创建 PR；现在让 Copilot 查找相关议题并关联 PR。

1. 在 Copilot Chat 面板中选择 **New Chat**。
2. 确认选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送以下提示：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示进行身份验证。
5. 确认 Copilot 找到了正确的议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时不要合并。

## 总结和后续步骤

本练习中，你了解了 MCP、在工作区添加了 Playwright MCP 服务器、使用浏览器测试筛选功能，并发布分支创建 PR。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
