---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成の指針を伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスを対象とする指示ファイルが、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot にどう伝えるかを確認します。
- 現在の指示に従ってフィルター用の最初のデータスライス (publishers ヘルパー) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うかを確認します。
- 次の演習で Copilot が更新したガイダンスを使えるように、指示の更新とフィルタースライスを `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作するためです。この演習では、出力を一字一句一致させるのではなく、指示の更新後に動作がどう変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインがあります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規約、ライブラリ、好みのコメント形式などを指定できます。リポジトリ全体、または特定の種類のファイル向けに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。大半のチャットや CLI リクエストに関係するプロジェクトレベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といった特定のタスクやファイル形式を対象にします。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

このワークショップで指示ファイルの作成を詳しく扱うことはありませんが、サンプルプロジェクトから一般的な方針を確認できます。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同様に、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] などのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。環境によって操作が異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要に応じて、その環境向けのレッスンで手順を説明します。
>
> テンプレートを探している場合は、指示ファイルやカスタムエージェントをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、`copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** には **GitHub Actions Workflows** の指針もあり、Copilot とのあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドは、リポジトリのルートからの相対パスとして、指示を適用する glob を指定します。ここでは `**/*.test.ts` に一致する TypeScript のテストファイルなどが対象です。
5. ユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、コード生成で参考にする例を指定できます (パスはリポジトリのルートではなく指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント要件はまだありません。次に TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以降の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順の中で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルでプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを見ます。

1. 前の演習で開いたエディタータブを閉じ、Copilot が参照するコンテキストを整理します。
2. `src/lib/publishers.ts` を開き、ヘルパーを作成する場所を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選び、ファイル編集を許可します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストなどを更新します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きの発行元配列を返す関数であることを確認します。これは `.github/instructions/drizzle.instructions.md` (`src/lib/*.ts` に適用) に記載されたデータ層の規約によるものです。
7. 提案されたコードには TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加する可能性があります。その場合も問題ありません。この演習の目的は、指示の更新後に一貫性がどう改善するかを確かめることです。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` を更新し、コード提案の質を高めるためにプロジェクト標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近の **Code formatting requirements** セクションを探します。現在、コード内ドキュメントの規則がないため、生成されたヘルパーにドキュメントコメントがありません。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の指示は `.github/copilot-instructions.md` に、言語やタスクに固有の指示は `*.instructions.md` に記載できます。TSDoc 規則のようなプロジェクト標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストを消去します。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同じようにソース管理します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎作成に Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けて pull request を作成します。GitHub ツールが接続されているので、Copilot に issue を探させて PR を作成・マージしてもらいましょう。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub ツールがワークスペースに設定済みです。初回の利用時に VS Code で GitHub へのサインインを求められた場合は、指示に従ってください。後の演習でこの接続方法 (Model Context Protocol) を学びます。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. 指示ファイル更新の issue を探して、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルター作業に使う publishers ヘルパーの作成を説明し、issue を PR でクローズするよう伝えます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して、許可してよい操作だけを承認します。
6. PR が作成されたら、PR を `main` にマージし、ローカルのブランチを `main` に戻すよう次のプロンプトで依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェルコマンドの実行を許可するよう求められたら、内容を確認して許可します。

GitHub Copilot の支援で pull request を作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、Copilot Chat を使って次の作業を行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うかを確認しました。
- 指示の更新とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は [エージェントモード][next-lesson]で、Copilot にコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどを集めたリポジトリ][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
