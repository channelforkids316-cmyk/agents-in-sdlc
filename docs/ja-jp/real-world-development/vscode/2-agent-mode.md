---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探し、変更し、テストを実行し、デバッグし、変更漏れを見つけて再度更新します。作業は続いていきます。

Copilot Agent Mode は、IDE 内でより自律的に作業するための機能です。開発者と同様にプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途です。エージェントモードは IDE 内で動作し、素早くフィードバックを得て対話しながら進められます。Cloud agent はチームメンバーのように非同期で作業し、issue と pull request を通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたって変更できること。
- UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリや発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。次は Copilot Agent Mode でデータ層、UI、テストの残りを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示する単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページはフロントマターから Drizzle ORM を介してローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. 前の演習の続きとして Codespace に戻ります。`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが準備できると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを公開する codespace のプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順の中で説明します。

サイトの初期実装は動作しますが、新しい機能を追加します。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に用意されています。GitHub Copilot にバックログを表示するよう依頼します。

1. Chat ビューのエージェント一覧から **Agent** を選択します。**Agent** は自律的に計画し、複数のファイルを変更して、ターミナルコマンドやツールを実行します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。より一貫した学習体験を意図していますが、LLM は確率的に動作するため、生成結果が説明と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な動作により、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択してすべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コードの生成を始める前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトの機能追加方法に関するコンテキストを含む [UI 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性、アクセシビリティに関する原則、Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンクがあります。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を小さく分け、例やひな形を示せます。

## フィルター機能を実装する

フィルター機能には、少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) のフィルターロジックを追加または調整します。
- フィルターの動作に関するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを追加する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。
4. モデルが **Claude Sonnet 4.5** であることを確認します。
5. 関連するバックログの issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを特定する様子を確認します。データ層のヘルパー、UI、テストを見つけてファイルを変更し、テストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行などを行うため、作業には数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。作業が終わると、すべての変更に対する操作が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで修正します。
9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を学びました。

- UI とデータ層にまたがる機能を実装しました。
- プロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更しました。
- コードベースにマージする前に、変更とテストをレビューする方法を確認しました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えたら、ページネーションが必要です。学んだ方法を使い、Copilot にページネーションを実装するよう依頼してみましょう。

- 指示ファイルなど既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- 必要に応じて Copilot に変更を依頼し、コンテキストを追加します。これは Copilot と作業する通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 各モードの機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
