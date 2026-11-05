---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」実行するかも伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスを対象とする指示ファイルによって、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に動作するため、リアルタイムで並べて効果を示すのは困難です。後続の演習で cloud agent が作成した pull request をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

優れた開発組織である Tailspin Toys には、開発手法に関するガイドラインと要件があります。次のような内容です。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発手法に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに適用する指示ファイルを作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。大半のチャットや CLI リクエストに関係するプロジェクトレベルのコンテキストを記載します。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象とする指針です。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といったタスクのガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく扱うのは、このワークショップの範囲外です。ただし、サンプルプロジェクトの例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。操作方法は利用する環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。必要に応じて、その環境のレッスンで手順を説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、`copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは、Copilot を使ったあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、リポジトリのルートからの相対パスとして、指示を適用するファイルの glob を指定します。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルなどが対象です。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで読みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。このように大きな指示を小さく再利用可能なファイルに分け、Copilot がコードを生成する際に参考にする例を示せます (ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、プロジェクト全体の標準を追加する作業だけです。コード内に TSDoc コメントとファイルレベルのコメントヘッダーを記載するルールを追加します。後続の演習で issue を割り当てると、cloud agent がこの指示を使用します。

編集の前にブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは変更をきちんとコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメント規則はありません。そのため、生成されたヘルパーにドキュメントコメントがなかったことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。これにより、ファイルコメントヘッダーと TSDoc コメントを追加するよう Copilot に指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、リポジトリレベルの `.github/copilot-instructions.md` には全般的な指示を、`*.instructions.md` には特定の言語、ファイル形式、タスクに関する指示を記載できます。TSDoc コメント規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチにある指示ファイルを読み取ります。次の演習では `main` からブランチを作成して Copilot に issue を割り当てるため、指示の変更を `main` にマージしておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で issue に割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを省略して、`main` に直接コミットできます。ここでは実際のプロジェクトに近い手順にするため、ブランチを作成しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスで cloud agent が作成する pull request をレビューするときに、この標準が適用されているかを確認できます。

次は [Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどを集めたリポジトリ][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
