---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルター機能を作成しました。pull request を開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) と、MCP サーバーによって Copilot にツールを追加する方法を学びます。
- ワークスペースに Playwright MCP サーバーを追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開して、フィルター機能の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに便利になります。Copilot が外部ツールやサービスを操作するための標準的な方法が Model Context Protocol (MCP) です。

![エージェントモードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

ツールやリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部のツールやサービス (既存の API や NPM パッケージなどのローカルツール) との通信を管理します。各 MCP サーバーは異なるツールとリソースを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの作成や更新、issue と pull request の管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内での発見や貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーもプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソースコードを確認し、公開元を検証して、セキュリティへの影響を慎重に検討します。信頼できる MCP サーバーだけを使い、機密情報や操作へのアクセスを許可する際は注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` サーバーを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` を編集し、`github` と並べて `playwright` を追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code がマシン上で `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、表示ウィンドウなしでブラウザーを実行するよう Playwright に指示します。デスクトップ環境がない codespace では、このオプションが必要です。

> [!NOTE]
> Tailspin Toys ではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーはすでにインストールされていることが多いです。後で Copilot がブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行するよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うと、VS Code がサーバーを起動し、信頼してよいか確認します。

1. `.vscode/mcp.json` の `playwright` の上に **Start** が表示されたら選択し、サーバーを起動して設定が正しいことを確認します。
2. サーバーを信頼するか尋ねられた場合は、設定を確認し、信頼して起動できるようにします。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを必要としたときに VS Code が起動し、信頼の確認を表示します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を確認して、内容をまとめます。会話を離れずに機能を確かめる方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を利用します。エージェントは、支援技術と同じようにボタン、リンク、リスト項目など、ラベル付きの構造化要素を基に動作します。簡単な機能確認が、アクセシビリティの基本チェックにもなります。

サーバーに接続してアプリを起動したら、次のプロンプトでフィルター機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告内容を issue の受け入れ条件と照らし合わせます。動作に問題があれば、pull request を作成する前に追加の質問をするか、コードを修正するよう依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合はプロンプトの送信前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードする場合があります。見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能の動作を確認できたら、チームでレビューする pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が作成した変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更されたファイルが表示されている Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## pull request を作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、pull request の作成方法はいくつかあります。GitHub Copilot を使って、関連 issue を検索し、関連付けた PR を作成してもらいましょう。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるように、エージェント一覧で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に、必要に応じて **Continue** を選択します。GitHub ツールの初回利用時に GitHub への認証を求められたら、手順に従って認証します。
5. Copilot が issue を検索して、適切な PR を作成したことを確認します。
6. Copilot が生成したリンクを選択し、pull request をレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、次の内容を行いました。

- Model Context Protocol (MCP) と、MCP サーバーによって Copilot にツールを追加する方法を学びました。
- ワークスペースに Playwright MCP サーバーを追加しました。
- 機能をリリースする前に、ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能の pull request を作成しました。

MCP サーバーで機能をテストしました。Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなど、対象を絞ったタスクを進めるために[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
