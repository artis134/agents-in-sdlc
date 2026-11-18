---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]用于指导代码生成。

本练习将学习如何：

- 通过仓库自定义说明和按路径限定的说明文件，将项目上下文、编码指南和文档标准提供给 Copilot。
- 使用当前说明生成筛选功能的第一部分（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 再次运行相同提示，观察生成代码如何应用新标准。
- 将说明更新和筛选代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有标准。Copilot 的输出具有不确定性。本练习旨在展示更新说明后行为的整体变化趋势，而不是要求输出逐字相同。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头应添加注释块，说明文件的用途。

通过说明文件，可以确保 Copilot 了解这些实践并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定编码约定、库和注释偏好，也可以针对整个仓库或特定文件类型创建说明。

- `.github/copilot-instructions.md`：每次针对仓库的请求都会发送给 Copilot 的说明文件，应包含技术栈、项目概述、最佳实践等项目级信息。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供说明，例如 TypeScript、Astro、UI 组件和单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体会使用仓库级说明，以及带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。此外，Copilot 也支持 AGENTS.md 和 CLAUDE.md 等其他标准的说明文件。

管理说明文件时，应遵循以下实践：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并说明代码应当（或不应当）如何编写。

说明文件没有唯一正确的写法，与使用 AI 一样，需要通过实验找到适合项目的方法。

> [!TIP]
> 可查看项目中的 [UI 更新][ui-instructions]和 [Astro][astro-instructions]说明。Copilot 也可以帮助生成说明文件，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。还可以在 [awesome-copilot][awesome-copilot] 中查找模板和示例。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

仓库提供了一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中查看。

1. 打开 `.github/copilot-instructions.md`，了解项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导；这些说明适用于与 Copilot 的所有交互。
2. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob 相对于仓库根目录；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
4. 阅读其中针对本项目单元测试的具体指导。
5. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部，查看它链接到的其他说明文件（例如 `unit-tests.instructions.md`）和项目文件。路径相对于说明文件，而不是仓库根目录。这样可以将较大的说明集拆分成较小、可复用的文件，并提供示例。

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 创建分支并在更新说明前使用 Copilot Chat

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

4. 返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；每一步会说明相关选项。
5. 关闭之前练习遗留的编辑器选项卡，避免 Copilot 获取不必要的上下文。
6. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应存放的位置。
7. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。
8. 使用以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数和测试。该辅助函数应是带类型的函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组；这些约定来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md`。同时注意，生成的代码可能缺少 TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有不确定性，即使没有要求也可能添加文档注释。这并无问题；重点是观察更新说明后行为的一致性是否改善。

## 添加新的仓库标准

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements**。这里记录了项目编码标准，但尚未包含代码文档要求，这也是辅助函数没有文档注释的原因。
3. 在现有标准正下方添加以下内容，要求 Copilot 为文件和函数添加文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。仓库级 `.github/copilot-instructions.md` 适合存放项目全局标准；特定语言、文件类型或任务的指导则放入 `*.instructions.md`。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，文件顶部现在有类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供用于从数据库检索发行商信息的函数。
    */
   ```

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

5. 注意，函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于后续练习会继续使用的代码。

## 提交并推送分支

1. 打开终端，运行 `git status`，确认说明更新和辅助函数更改均存在。
2. 暂存并提交说明更新及辅助函数文件；如果状态中还显示测试等相关更改，也一并暂存：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他相关更改（例如测试），也一并添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

仓库模板已将 GitHub 工具连接到工作区。首次使用时，VS Code 可能会提示登录 GitHub；按照提示完成授权。稍后会在 MCP 练习中了解此连接方式。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并在智能体选择器中确保选择 **Agent**。
3. 要求 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，同时说明 PR 会关闭该议题，也包含为后续筛选工作添加的发行商辅助函数：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。系统提示允许 Copilot 代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
5. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. 如果系统提示允许 Copilot 代表你执行 GitHub 操作或运行 shell 命令，请先审查命令，再酌情选择 **Allow**。

## 总结和后续步骤

你了解了 Copilot 如何获取说明文件中的上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有说明的影响。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。
- 再次发送相同提示，观察生成的代码如何应用标准。
- 将说明更新和辅助函数合并到 `main`，供后续练习使用。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
