---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に伝える背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」実行するかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムに並べて効果を示すのは困難です。このハーネスの後半で cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

開発会社である Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには、必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な印象にします。
- TSDoc のドキュメント コメントを使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングのスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにできます。推奨するコーディング規則、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイル種別に指示を設定できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティスなど、多くのチャットや CLI のリクエストで参照するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、TypeScript などの言語や UI コンポーネント、ユニット テストなど、特定の言語、ファイル種別、タスクに関する指示を記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターで適用先を指定した `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルを作成する方法をすべて説明することは、このワークショップの範囲外です。サンプル プロジェクトの例では、代表的な方法を紹介しています。基本として、次の点に注意してください。

- `copilot-instructions.md` には、作成するものの説明、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ レイヤーなどのファイル種別や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスと、コードのよい例・避けるべき例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトで試して、適切な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、インターフェイスごとに方法が異なります。該当する操作は、各ハーネスのレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスを含め、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの glob で適用対象を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、指示を再利用しやすい小さなファイルに分け、生成時に参照する例を示せます (パスはリポジトリのルートではなく指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント要件はまだありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## リポジトリの標準を追加する

次は、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーをコードに記載する、プロジェクト全体の規則を追加します。このハーネスの後半で Issue を割り当てると、cloud agent はこの指示を使います。

編集する前に、作業ブランチを作成します (cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を整理してコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト レベルの情報を記載し、コード提案の改善に役立つコーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内のドキュメントの規則がまだないことを確認します。
3. 次の Markdown を既存の標準の直後に追加し、ファイル コメント ヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体に適用する指示の保存先であり、`*.instructions.md` ファイルは特定の言語、ファイル種別、タスクに適用する指示の保存先です。ドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルを使います。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てるとき、Copilot は `main` からブランチを作成します。そのため、指示の変更を `main` にマージしておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てる Issue を処理する際、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず `main` に直接コミットしてもかまいません。ここでは実際のプロジェクトに近い手順にするため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。cloud agent がこのハーネスの後半で作成するプルリクエストに、その標準が反映されます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [指示ファイルなどを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
