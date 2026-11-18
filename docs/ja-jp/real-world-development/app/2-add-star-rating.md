---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを試したので、次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。各ゲームのデータには星評価がありますが、ホームページのゲーム カードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構造を学びます。
- エージェントに小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲーム カードには、タイトル、カテゴリ、パブリッシャー、説明しか表示されていません。最初のセッションでは、各カードに既存の評価を表示します。これは小さく独立した変更なので、最初のセッションに適しています。

## セッションの構造

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーでリポジトリごとにグループ化され、選択して切り替えられます。

セッション内では、エージェントとの**会話**、ファイルの調査や編集で行われる**ツール操作**、差分とともに表示される**変更ファイルの一覧**を確認できます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを確認して機能の実装に取りかかります。[アプリの設定][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻ります (またはアプリを開きます)。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選びます。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選びます。次のプロンプトで変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能に必要なファイルを特定してコードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージする前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選ぶと、変更されたファイルと差分が表示されます。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> すべての生成 AI ツールと同様に、Copilot の動作は確率的です。実際のコードは上記の例と異なる場合がありますが、比較的似た形になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動実行したチェックの結果をレビューします。テストで数値の `starRating` と、`null` の場合の代替表示がカバーされていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功とはみなせません。インストールの要求は内容を確認してから承認します。

コードを読んだだけで動作すると判断せず、Copilot に Web サイトを起動させ、ブラウザー キャンバスで更新された UI を確認します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム Canvas を調べ、自分でも作成します。ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトで、アプリを起動してブラウザー Canvas で開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに 5 点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッションが起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、コードベースに変更をマージするためのプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. PR の作成に対応する Issue が必要だと Copilot が説明した場合は、その手順に従うよう指示します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。ここで必要に応じてレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って、最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、その構造を学びました。
- ゲーム カードに小さく対象を絞った変更を加えるようエージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は、[フィルタリングの Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を作成します。

## リソース

- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
