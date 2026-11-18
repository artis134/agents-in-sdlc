---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、複数のエージェントをそれぞれ独立したワークスペースで並列に指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールしてプロジェクトを用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログなど、ワークスペースを確認します。
- クイックチャットを試して、アプリについて学びます。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンではアプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

Windows、macOS、Linux で利用できる GitHub Copilot app をインストールし、認証して Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディング ページの指示に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com アカウントでサインインします。データ所在地に対応する GitHub Enterprise Cloud を使う場合は **GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使用する場合は、まずクラウド アカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインイン ガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディングでリポジトリを選択する画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択を省略して、オンボーディングの後でプロジェクトを追加することもできます。
6. テーマを選ぶよう求められたら、テーマを選択してオンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されているか確認します。すでに表示されている場合は、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. サイドバーにリポジトリが表示されることを確認します。ここで使用するレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーには主に次の領域があります。

- **New** - 新しい Copilot チャット セッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて、プルリクエストを確認できます。アプリを離れずに参照、絞り込み、CI ステータスの確認、レビューを行えます。
- **Issues** - Issue を参照して絞り込み、Issue からセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。To Do リストの管理、定期的な保守などを任せるのに便利です。まとめでは次のステップとして紹介し、ここで追加の演習は行いません。
- **Customize** - MCP サーバー、プラグイン、スキルなどを使って Copilot app に機能を追加します。Playwright MCP の設定に使用します。
- **Chats** — ブランチや専用ワークスペースを必要としない、質問やブレインストーミングのための軽量な会話です。最初のチャットを開始すると表示されます。表示されない場合は **Sessions** の横の **+** を選び、チャットを開始してください。
- **Sessions** — リポジトリ フォルダーとエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。セッションごとに独立したワークスペースを使うため、変更を競合させずに複数のセッションを実行できます。最初のセッションは星評価を追加する際に開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートを作成した際に用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** の下で、個々のセッションではなく Tailspin Toys のリポジトリ フォルダーを選びます。その後、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue からエージェント セッションを開始できます。小さな最初の変更が終わったら、フィルタリングの Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。別のリポジトリの作業項目を確認するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリの使い方に慣れることができます。**クイックチャット**では、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選び、新しいチャットを開きます。**Chats** がまだ表示されない場合は、**Sessions** の横の **+** を選んで最初のチャットを開始します。チャットの後に **Chats** が表示されます。
2. アプリでセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで応答を読みます。各セッションが専用の git worktree で実行されるため、変更を競合させずに複数のエージェントを並列で動かせることがわかります。会話は続けることも、新しいチャットを開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを探す。
- クイックチャットを使って簡単な質問をする。

次は、[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使い始め方][getting-started]
- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
