---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリングの検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、単にコードを書く以上のことが含まれます。コードが動作することは手動で検証し、指示ファイルを使って標準に従っていることも確認しました。では、テストや lint、継続的インテグレーション (CI) のその他の作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が最適です。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の報告形式をカスタマイズします。
- スキルを実行し、出力をレビューします。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、エージェントが必要に応じて読み込む、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、スキルの内容を定義する `name` と `description` のフロントマター、概要、呼び出すタイミングのガイダンスを記載します。スキルが使うスクリプトやその他のリソースをサブフォルダーに含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。この例では既存の `npm` コマンドを使ってテストと linter を実行するため、追加の補助ファイルは必要ありません。

スキルは、チームで共有・再利用できるようプロジェクトの `.github/skills` に配置するか、通常は `~/.copilot/skills` にあるユーザーのスキル フォルダーに配置できます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. レビュー パネルに **Files** Canvas が開いていない場合は、**+**、**File** の順に選択します。
2. `.github/skills/quality-checks/SKILL.md` を検索します。
3. 先頭の `name` と `description` を読みます。説明によって、Copilot はスキルを呼び出すタイミングを判断します。
4. 指示を読み、テストと lint のプロセスを Copilot に案内する方法を確認します。

## 変更前にスキルを実行する

スキルはスラッシュ (`/`) コマンドから直接、または自然言語を使って呼び出せます。まず 3 種類のチェックを実行するよう Copilot に依頼します。

1. モードのドロップダウンから **Interactive** を選び、Copilot が Interactive モードであることを確認します。
2. 次のプロンプトでスキルを呼び出します。

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

どのチェックを実行したか、成功したか、ツールが実際に報告した内容をわかりやすく示すレポートにします。スキルを更新しましょう。

1. **Files** Canvas に戻ります。
2. まだ開いていない場合は `.github/skills/quality-checks/SKILL.md` を開きます。
3. 次のセクションをファイルの末尾に追加します。

    ```markdown
    ## Results output formatting

    Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
    ```

ファイルは自動的に保存されます。

## 更新したスキルを実行する

スキルを更新したので、先ほどとまったく同じプロンプトを使って試してみましょう。

1. モードのドロップダウンから **Interactive** を選び、Copilot が Interactive モードであることを確認します。
2. 次のプロンプトでスキルを呼び出します。

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. 最後に出力されるレポートを確認します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックのための `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを実行して出力をレビューしました。

この変更は、フィルタリングの機能 PR に含まれます。次は、[Playwright MCP サーバー][next-lesson]を介して Copilot にサイトを直接操作させます。

## スキルの例

以下のコミュニティの例は参考資料であり、追加作業ではありません。採用する前に、前提条件と動作を確認してください。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

上流のコントリビューション例の名前は `make-repo-contribution` です。古い Tailspin テンプレートでは、異なる名前の `make-contribution` を使っていました。このワークショップは、どちらのコントリビューション スキルにも依存しません。

[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
