---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将学习如何：

- 通过仓库自定义说明和按路径限定的说明文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会比较更新说明前后的提示效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示差异。你将在后续练习中审查云智能体创建的拉取请求，观察说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头应添加注释块，说明文件的用途。

使用说明文件可以确保 Copilot 了解这些实践，并据此完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。你可以指定编码约定、库以及注释风格，也可以为整个仓库或特定文件类型创建说明，为任务提供上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该仓库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的文件，可为 TypeScript、Astro、UI 组件或单元测试等提供指导。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot 云智能体都会在生成代码时使用仓库级说明和带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。Copilot 也支持其他标准的说明文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对文件类型或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码示例。

说明文件没有唯一正确的写法，与使用 AI 一样，需要通过实验找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的说明文件集合。可以查看项目中用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的说明。Copilot 也能帮助生成说明文件；不同界面提供的操作不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。也可以在 [awesome-copilot][awesome-copilot] 中查找模板和示例。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义说明文件

阅读仓库自带的核心 `copilot-instructions.md` 和各类任务对应的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`，了解项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意嵌套的 **GitHub Actions Workflows** 指导。
2. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它使用相对于仓库根目录的 glob；例如匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用该说明。
4. 阅读其中针对本项目单元测试的指导。
5. 打开 `.github/instructions/drizzle.instructions.md` 并查看末尾，了解如何链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目文件。这样可以将较大的说明集拆分成可复用文件，并为 Copilot 提供示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

先创建分支。云智能体会读取议题目标分支中的说明，但仍应以清晰的方式提交本次更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`，找到大约位于第 27 行的 **Code formatting requirements** 部分。
3. 在现有标准下方添加以下规则：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 仓库级 `.github/copilot-instructions.md` 适合放置项目级标准；`*.instructions.md` 则适合提供特定语言、文件类型或任务的指导。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一练习中，Copilot 会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体处理分配给它的议题时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 上完成本工作坊，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目的处理方式。

## 总结和后续步骤

你已了解 Copilot 如何获取项目说明中的上下文，并在 `.github/copilot-instructions.md` 中添加了仓库级标准。后续练习中，云智能体创建的拉取请求会体现该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
