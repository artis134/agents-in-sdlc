---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールは複数あります。GitHub.com の[エージェント ページ][agents-page]もその 1 つです。この Mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに方向を示したりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイ コントラスト モードを作成するようエージェントに割り当てた後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、これも含めることが適切です。ハイ コントラストに加え、ライト モードも実装するようエージェントの作業を案内します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にあるタスク一覧を確認します。割り当てたタスクとして、コードのドキュメントの更新、ゲームを変更する API の生成、Web サイトへのハイ コントラスト モードの追加などが表示されます。
3. 実行中のタスクを 1 つ選び、Copilot が実行した操作を確認します。たとえば、リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための各手順などです。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、進行状況を監視できます。

## Cloud agent に作業の方向を示す

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選びます。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページの Copilot タスクに表示される、実行中のセッションへの追加指示ダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込むことを確認します。

## Copilot に作業を任せる

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中の手順を終えると、新しいリクエストを作業に反映します。

作業には数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent タスクの監視と案内に使う中心的な場所として、Copilot agents ページを確認しました。Mission control では、実行中および完了したタスクの追跡、作業内容のレビュー、進行中タスクへの追加指示ができます。

このレッスンでは、次のことを行いました。

- Mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに指示を追加し、機能を増やしました。

Copilot がアクセシビリティ機能を完成させる間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
