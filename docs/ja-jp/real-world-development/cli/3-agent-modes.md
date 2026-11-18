---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で Issue に基づくフィルタリングを実装して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず、小さな機能をプロジェクトに追加しました。しかし、大きな変更には、より確実なプロセスが必要です。GitHub Copilot CLI は、組織に既存の開発フローがある場合でも、それに沿って適切なものを適切な方法で構築できるように設計されています。このレッスンから数回にわたり、Issue を起点に機能を実装し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージする、一般的なエージェント主導の開発プロセスを学びます。

> [!NOTE]
> 機能のワークフローでは、同じ会話とブランチを続けて使用します。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動で検証します。

この機能の開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリやパブリッシャーでゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで方針を決め、その後、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がさらに重要になります。多くの開発者は、次のようなフローで作業します。

1. 実施内容を記載した Issue を起点にします。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 正確な手順はチームや組織によって異なりますが、ほとんどの開発フローは上記の流れを調整したものです。

この標準的な方法に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントがどの程度自律的に作業するかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押し、**Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての Issue を表示します。
8. 矢印キーを使い、**Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルタリング機能を計画する

計画を立てることで、Copilot に作業を任せる前に、機能の実装方針やタスクの進め方を決められます。複雑な作業では、少し時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリング機能の動作について質問されたら、希望する内容を回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決定した内容と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot が完了したと判断するまで問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様の名前の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。まず計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様に、マージ前にレビューします。コードを確認し、サイトを実行して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** も **Autopilot** も表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画で決めた内容と照らし合わせます。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードの確認ができたら、アプリが動くことも確かめましょう。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. 次のプロンプトでアプリを起動するよう Copilot に依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリとパブリッシャーのフィルターや組み合わせを試します。
4. リセット時と検索結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動で検証しました。

次は、コード生成をさらに詳しく確認し、記載されたプラクティスに沿うように、[カスタム指示を使用します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
