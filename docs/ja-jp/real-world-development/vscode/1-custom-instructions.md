---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に伝える背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルでコード生成のガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 既存の指示を適用した状態で、フィルタリングに使う最初のデータ スライス (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の変更とフィルタリングの基礎となるコードをコミットして `main` にマージし、次の演習で新しいガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、出力が文字単位で同じになることではなく、指示を更新した後の傾向の変化を確認します。

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

## プロジェクトのカスタム指示ファイルを確認する

リポジトリに含まれる、中心となる `copilot-instructions.md` とタスクごとの `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも含め、このリポジトリでの Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの glob で適用対象を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより、指示を再利用しやすい小さなファイルに分け、Copilot が参照する例を示せます (パスは指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント要件はまだありません。次の手順で TSDoc コメントとファイル ヘッダーの規則を追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルの状態でプロンプトを送ります。その後、指示を更新して、同じプロンプトをもう一度送ります。

1. 前の演習で使ったエディター タブを閉じ、Copilot が参照するコンテキストを絞ります。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント一覧で **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べて、ヘルパーとテストを含むファイルを更新します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、パブリッシャーの型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規則によるものです。
7. 生成されたコードには、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることがこの演習の目的です。

## リポジトリの標準を追加する

`.github/copilot-instructions.md` に、コード提案の改善に役立つプロジェクト レベルのコーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクション (27 行目付近) を探します。コーディング標準は記載されていますが、コード内ドキュメントの規則はないことを確認します。
3. 次の Markdown を既存の標準の直後に追加し、ファイル コメント ヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体に適用する指示の保存先であり、`*.instructions.md` ファイルは特定の言語、ファイル種別、タスクに適用する指示の保存先です。ドキュメント コメントの規則のようなプロジェクト全体の標準には、リポジトリ レベルのファイルを使います。

## プロンプトをもう一度実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んでバッファーをクリアします。
2. `src/lib/publishers.ts` を再度開き、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習でも使う実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同様に、ソース管理で管理します。変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基礎コードについて、Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなど追加の関連ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue に関連付けてプルリクエストを作成します。手動で行う代わりに、Copilot に Issue を探して、Issue を解決する PR を作成し、マージしてもらいます。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクト テンプレートには GitHub ツールがすでに設定されています。最初の利用時に VS Code で GitHub へのサインインを求められた場合は、画面の指示に従います。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう、次のプロンプトで依頼します。指示の更新と、今後のフィルタリング作業のために追加したパブリッシャー ヘルパーを説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の実行許可を求めたら、コマンドを確認し、必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェル コマンドの実行許可を求めたら、内容を確認して必要に応じて **Allow** を選択します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示の状態でコード生成プロンプトを送り、提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトをもう一度送り、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、指示とヘルパーの基礎コードをコミットして `main` にマージしました。

次は、指示を[エージェント モード][next-lesson]で使い、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [指示ファイルなどを集めた Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
