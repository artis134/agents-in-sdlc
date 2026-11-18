---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを特定して変更し、テストを実行して問題をデバッグします。見落としたファイルに気付くと、さらに変更を加えます。このような作業を繰り返します。

Copilot エージェント モードは、このような作業を支援します。

Copilot エージェント モードは、IDE 内でより自律的に作業するように設計されています。開発者と同じように既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェント モードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けに作られています。エージェント モードは IDE 内で作業し、すばやいフィードバックとやりとりを実現します。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやりとりします。

この演習では、次のことを学びます。

- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- UI とデータ レイヤーにわたって GitHub Copilot エージェント モードで機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャー ヘルパーを追加しました。次は Copilot エージェント モードで、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の機能を理解するために Tailspin Toys の Web サイトを確認しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリであり、ビルド時に静的 HTML を生成します。ページはフロントマターで Drizzle ORM を通じてローカル SQLite データベースを直接照会します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブ機能には標準 DOM API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一 Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか確認するダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に移動します。これは、Web サーバーを実行している Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定した際に Issue が自動的に用意されました。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選びます。**Agent** はファイルをまたぐ変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨する意図はありません。作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。受け取るコード提案の一貫性を高めることが目的ですが、LLM の動作は確率的なので、例と異なる提案になることがあります。これは通常の動作です。

> [!NOTE]
> 確率的な性質により、Copilot が異なる MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選び、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目から順に優先度を付けたことを確認できます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトへの機能追加の方法を説明する[ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計、テスト容易性とアクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル種別に関する指示へのリンクが含まれます。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、小さく管理しやすいファイルに分け、例やひな形を示せます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ レイヤー (`src/lib/`) に残りのフィルタリング ロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させます。Copilot エージェント モードにこれらの作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選んで新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選び、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue を参照して機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを特定する様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示されることがあります。完了するとすべての変更に対する **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot のプロンプトが表示されたら、**Continue** を選んでテストを実行します。

   ![Copilot Chat でテスト実行の確認を求める画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、テストに失敗したりすることがあります。Copilot は、タスクを完了してエラーがないと判断するまで、コード生成とテストを反復します。

   ![Copilot エージェント モードのチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードをレビューすることは、常に重要です。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. すべてが動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードで Tailspin Toys の Web サイトに機能を追加する方法を学びました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーにまたがる機能を実装できます。
- Copilot エージェント モードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を実行できます。
- コードベースにマージする前に、生成された変更とテストをレビューできます。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼し、コンテキストを追加して反復する必要がある場合があります。これは Copilot と作業する通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
