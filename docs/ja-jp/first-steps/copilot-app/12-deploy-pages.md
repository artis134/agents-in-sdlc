---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを設定し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開し、誰でもアクセスできるようにします。Copilot はワークフローを準備してデプロイを案内できますが、変更をレビューし、サイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイ設定を依頼します。
- ワークフローとプルリクエストをマージ前にレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ誰でも簡単に試せる状態ではありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイムの依存関係を追加せず、公開 URL からアクセスできるようになります。

## Copilot にデプロイの準備を依頼する

> [!WARNING]
> プルリクエストの作成によってコードがコミットおよびプッシュされることがあります。プロンプトを送る前に、承認設定と送信先リポジトリを確認してください。**Approve all** を選択している場合、個別の確認が表示されるとは限りません。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローとプルリクエストをレビューする

1. README は、リポジトリに存在しない場合にのみ追加されていることを確認します。
2. ビルドとデプロイの手順、トリガー、権限を含めて、プルリクエストの変更をレビューします。
3. ワークフローが、マージされていないプルリクエストではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新された差分を確認します。マージを許可する前に、必要なプルリクエストのチェックが成功するまで待ちます。

## マージとデプロイを許可する

> [!WARNING]
> 次のプロンプトを送ると、レビュー済みのプルリクエストのマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリとプルリクエストを確認してから送信してください。承認設定によっては、これらの操作が追加確認なしで実行されます。

レビューしたプルリクエストの番号を `<PR_NUMBER>` に置き換えてから、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![マージされた Space Quiz のプルリクエスト、Build website、Upload artifact、Deploy to Pages の各手順が成功した GitHub Actions の Deploy website 実行、yourname.github.io/space-quiz/ で公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages の有効化やプルリクエストのマージを行えない場合は、示された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、タスクを完了したと判断しないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告だけに頼らず、デプロイを確認しました。次は、[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
