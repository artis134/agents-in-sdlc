---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次のことを学びます。

- コード生成のための環境をカスタマイズする。
- 操作が安全に行われるようにする。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys は、いくつかの技術的負債を解消したいと考えています。最初のサイトを開発した受託業者が、ドキュメントを十分に作成していませんでした。まず、アプリケーション内のエクスポートされた関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルを「関連するゲーム」として表示し、閲覧を続けられるようにします。まだ完成度の高い実装は不要で、UX の受け入れテストに使えるものを求めています。この作業はブロッカーですが、現時点では優先度の高い Issue がほかにあります。

このように、優先度を下げられがちな作業は Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent は非同期に作業するため、開発者は別のタスクに集中し、あとから作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]られます。割り当てると Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに着手します。実行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作にも、詳しく調べると複雑な部分があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に定めてください。AI ペア プログラマーとして扱い、段階的に進めて学び、試し、適応させます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot の動作を導く方法を学びました。カスタム指示の実習を終えていれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、このタイミングで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後からその影響を見つけられるようにします。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規則と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。`**/*.test.ts` ファイルに指示が適用されます。

次のセクションで **Code lacks documentation** という Issue を cloud agent に割り当てると、作成されたプルリクエストに TSDoc コメント、TypeScript の規則、ファイル ヘッダーが反映されているか確認できます。後の演習で PR をレビューする際に、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

コードを作成する際は、通常、必要な開発ツールや依存関係を利用できるよう環境を準備します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業するため、環境のセットアップが必要です。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールと依存関係を利用できるようになります。ラボの流れを円滑にし、この仕組みを学べるよう、あらかじめ設定済みです。Cloud agent が Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、単一 Astro アプリ用にマイグレーションとシードを適用した SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、多くのプロジェクトで情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性を下げたり、コードの保守や新しい開発者の参加を難しくしたりします。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適したタスクです。バックグラウンドで必要なドキュメントを作成し、後の演習でレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選びます。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選びます。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選びます。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot の To Do リストが表示されます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、最適な進め方を検討しています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ウィンドウの再読み込みが必要な場合があります。

16. セッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断し、コードを作成していく過程を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを実行して、ほかの作業に集中できるようにすることです。この機能を活用し、別のタスクも Copilot cloud agent に割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チーム向けに関連ゲームを追加するのに時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選びます。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選びます。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てのダイアログを開きます。

   ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が Issue の最初のコメントに表示されます。

![Issue に Copilot の作業中を示す目の絵文字が表示されている画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェア エンジニアと同じように動作するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、非同期に Issue を処理できることを学びました。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを Copilot に任せられます。

このレッスンでは、次のことを行いました。

- コード生成のための環境をカスタマイズしました。
- 操作が安全に行われるようにしました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

Cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントの作成と使用に取り組みます。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]とカスタム指示も利用できます。これらの指示は前のモジュールで確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
