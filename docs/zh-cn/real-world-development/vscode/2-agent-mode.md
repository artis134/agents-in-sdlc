---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员需要找到相关文件、进行更改、运行测试、调试、发现遗漏并再次修改。Copilot 智能体模式可以更自主地处理这些工作：先探索项目结构，再协调更新、运行任务并修复发现的问题。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同场景。智能体模式在 IDE 中执行，可快速交互；云智能体则通过议题和拉取请求异步工作，类似团队成员。

本练习将学习 Copilot 智能体模式如何探索项目、识别文件、跨 UI 和数据层实现功能，以及如何审查更改和测试。

## 场景

随着游戏列表不断增长，希望用户能按类别和发行商筛选。上一练习已添加发行商辅助函数，现在使用 Copilot 智能体模式完成数据层、UI 和测试。

## 运行 Tailspin Toys 网站

Tailspin Toys 是面向开发者的桌游众筹网站。它是一个 Astro 应用，构建时生成静态 HTML 页面；页面通过 Drizzle ORM 在前置元数据中查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用标准 DOM API 编写的小型 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回 codespace，确认当前位于 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

5. Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

6. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），再选择终端中的 Astro 服务器地址 `http://localhost:4321`。在 Codespace 中，该链接会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`，这是托管 Web 服务器的私有隧道。

### 浏览网站

网站运行后，查看主要功能：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**。各练习所需模式和模型可能不同，步骤中会注明。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中显示智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)
2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入步骤中，以便尽量保持建议一致。LLM 具有概率性，收到不同建议完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，询问 Copilot 仓库中的议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看 Copilot 生成的议题列表。它还会根据对用户的帮助程度为条目排序。

## 审查说明文件

开始生成代码前，先查看用于为 Copilot 提供上下文的 `.github/instructions/ui.instructions.md` 文件：

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加网站功能的总体指导，包括架构概述、组件设计、可测试性和无障碍访问原则，以及指向 Astro、Tailwind CSS 和 Drizzle 文件类型说明的链接。

> [!TIP]
> 说明文件可以引用其他说明和项目文件，路径相对于说明文件所在位置。这有助于拆分复杂指导、重复使用内容并提供示例。

## 实现筛选功能

实现筛选至少需要对应用做三项单独更改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面，加入筛选 UI。

合并前还需要运行测试并确保通过。Copilot 智能体模式可以处理这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat**。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认选择了 **Agent** 和 **Claude Sonnet 4.5**。

   ![Chat 视图中显示智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送提示：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，具体过程可能因任务复杂度和代码库而需要几分钟。期间编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示这两个选项；工作尚未完成时无需选择。

6. 按 Copilot 提示选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

7. 过程中可能会有暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且不再发现错误。

   ![包含 Copilot 智能体模式完整会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

8. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

9. 返回运行着网站的浏览器，体验新功能。
10. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能，并学习了如何：

- 跨 UI 和数据层实现功能。
- 探索项目、识别文件并协调更改。
- 在合并前审查 Copilot 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 加分练习 — 实现分页

随着游戏列表增长，网站需要分页。使用本练习学到的技能提示 Copilot 更新网站。考虑沿用说明文件和现有最佳实践、决定页大小由用户选择还是固定，以及为 Copilot 提供足够上下文。根据需要迭代提示和实现，这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
