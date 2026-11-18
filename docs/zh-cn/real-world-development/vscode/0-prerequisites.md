---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要完成准备工作。你将创建 Tailspin Toys 仓库副本、启动一个用于实验的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 可以正常使用。

## 设置实验仓库

从[模板][template-repository]创建仓库实例。新仓库包含实验所需文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面选择 **Use this template**，再选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加 GitHub 或 Microsoft 主办的活动，请遵循导师的说明。否则，可在有权使用 GitHub Copilot 的组织中创建仓库。
4. 记下所创建仓库的路径（**organization-or-user-name/repository-name**），稍后会用到。

   ![输入仓库模板设置](../../../_images/ex0-repository-settings.png)

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，系统会自动创建 GitHub 议题待办列表。整个工作坊都会使用这些议题，无需自行创建。

## 创建 codespace

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码，并提供支持多种语言、扩展和工具的完整 IDE。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。
3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

   ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习会再次使用此 codespace。现在请将其保留在浏览器选项卡中。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。若要在本地运行，请在 VS Code 中打开克隆的仓库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 在 VS Code 中使用 GitHub Copilot Chat 和智能体模式

使用 GitHub Copilot Chat 智能体模式需要安装 GitHub Copilot Chat 扩展。GitHub Codespace 通常已预装此扩展。

> [!TIP]
> 如果尚未安装，可从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装；也可打开 VS Code 的“扩展”视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装后，可能需要使用 GitHub 账户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧未显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入 “Hello world” 等消息并按 Enter。
4. 如果尚未通过身份验证，按照提示登录 GitHub 账户。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示。

## 总结

恭喜，你已创建实验仓库副本并开始创建 codespace，之后会在其中编写代码。

## 下一步

继续学习[练习 1 - 自定义说明][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
