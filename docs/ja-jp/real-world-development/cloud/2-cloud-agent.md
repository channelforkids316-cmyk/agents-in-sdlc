---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行します。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発者はより大きな課題に集中し、開発を速められます。

Copilot cloud agent では、次の内容を学習します。

- コード生成の環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- issue の範囲を明確にする重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した担当者がドキュメントを残しておらず、アプリケーションにはほとんどドキュメントがありません。まず、エクスポートされているすべての関数に TSDoc のドキュメントコメントを追加します。

デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのゲームを表示し、ユーザーが続けて閲覧できるようにします。洗練された実装はまだ必要ありません。受け入れテストで UX を試せるものが必要です。これは現在のブロッカーですが、さらに優先度の高い issue がほかにあります。

このようなタスクは優先順位が下がりやすく、Copilot cloud agent に任せるのに適しています。Copilot cloud agent は非同期に作業するため、開発者はほかの作業に集中できます。後で作業内容をレビューして、期待どおりの結果になっているか確認します。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法などがあります。割り当てると、Copilot は進行状況を追跡するドラフト pull request を作成し、環境を準備して、タスクに取り組みます。作業中でも完了後でもセッションを確認できます。提案された解決方法のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省くことはできません。一見単純な操作にも、細部を確認すると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階を踏み、学び、試し、改善します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot をガイドする方法を確認しました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が動作する様子を見ています。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、変更せずに指示ファイルを読み、後で成果物に反映されているかを確認できるようにします。

GitHub Web UI または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、これらの指示ファイルに基づく TSDoc コメント、TypeScript 規約、ファイルヘッダーを確認します。後の演習で PR をレビューするときに、改めて説明します。

## Copilot cloud agent の開発環境を準備する

誰がコードを書く場合でも、適切な環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent は SWE と同じように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[セットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係をインストールできます。演習の準備として、このファイルは設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用ブラウザー、単一 Astro アプリ用にマイグレーションと初期データ投入を済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このファイルは GitHub ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR の作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあり、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が作業を始めるのを待たずに、正常に実行されるか試せます。

## ドキュメントを追加する

ドキュメントは重要ですが、情報が古かったり、まったくなかったりするプロジェクトもあります。技術的負債として放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメント作成が得意なため、cloud agent に依頼するのに適した作業です。必要なドキュメントをバックグラウンドで作成させ、後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択します。
8. 右側の **Assign to Copilot** を選択します。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を作成して (前述のワークフローを実行)、プロジェクトを分析し、問題への最適な取り組み方を考えています。

13. todo リストと、実行予定のタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態の更新を確認するには、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調べて状態を理解し、計画を考えて、コードを作成するまでの動きを確認できます。

これには数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを実行して別の作業に集中できるようにすることです。もう 1 つタスクを割り当てたら、アプリケーションの機能を実装する作業を進めます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザインチーム向けに関連ゲームを追加する作業を任せて、その間にほかのタスクに取り組みます。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

  ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が issue の最初のコメントに表示されます。

![issue で Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常に監視する必要はありません。完了後にレビューすればよいのです。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を Copilot に割り当てて非同期に処理できます。技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次の内容を学習しました。

- コード生成の環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- issue の範囲を明確にする重要性を学びます。
- issue を Copilot に割り当てます。

cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールでは、カスタム指示の使い方も学びました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
