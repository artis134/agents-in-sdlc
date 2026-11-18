---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションで、エージェント主導の開発を単一の作業用ワークスペースで実現します。並列エージェント セッション、切り替え可能なセッション モード、共有 Canvas、GitHub Issue とプルリクエストのネイティブ管理機能を備えています。さらに **Agent Merge** を使うと、リベース、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを含めて、プルリクエストを完了まで進められます。

このワークショップでは、Tailspin Toys で一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーで確認し、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始めて、**Plan** モードで方針を定め、**Autopilot** モードで実装し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルタリングを確認します。
7. QA カスタム エージェントを作成して使い、要件、カバレッジ、検証結果をレビューします。
8. フィルタリングの変更全体をレビューし、2 つ目の PR に Agent Merge を使用します。
9. Canvas が利用できるか確認し、Database Explorer を使います。その後、リポジトリに保存するトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめのレッスンに進みます。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの 2 つの PR を作成します。それぞれ更新済みの `main` から始めます。フィルタリングと品質のワークフローでは、1 つのセッション、worktree、ブランチを共有し、各ツールを学びながら作業を積み上げます。最後の Canvas 演習も同じセッションで進めます。PR の作成手順を繰り返さず、共有サーフェスの作成とテストに集中できます。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と評価がない場合の表示を追加し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認・更新し、フィルタリングに適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を設定し、フィルタリングの動作を調べます |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終的な検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使用し、リポジトリに保存するトリアージ Canvas を作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、関連リソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合でも、認定された学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれるため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで実行します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI のポリシーとは別です。

## 始める

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
