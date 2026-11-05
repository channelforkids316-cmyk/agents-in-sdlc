---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[**GitHub Copilot app**][about-copilot-app] は、エージェント主導の開発に使用するデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。すべての作業を自分で行うのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並列に指示し、繰り返し発生するタスクを自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーが完了したので、次はアプリをインストールして、そのリポジトリを接続します。

このレッスンでは、次の内容を学習します。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットを試して、アプリ自体について学ぶ。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、pull request のマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) のいずれかが必要です。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使用するには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストールが完了したら、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を設定して GitHub Enterprise Cloud を使っている場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリを選ぶ画面が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリ選択をスキップし、後からプロジェクトを追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧にある場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. サイドバーにリポジトリが表示されることを確認します。このレイアウトでは `tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、各領域を確認します。アプリのサイドバーは、主に次の領域で構成されています。

- **New** — Copilot との新しいチャットセッションを開始します。
- **Pull requests** — アプリの GitHub ネイティブ統合を通じて表示されます。アプリを離れずに pull request の参照や絞り込み、CI ステータスの確認、レビューができます。
- **Issues** — GitHub issue を参照、絞り込み、選択して、issue からセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する保存済みエージェントタスクです。やることリスト、定期メンテナンス、その他の定型作業に役立ちます。振り返りでは次のステップとして紹介し、別の演習にはしません。
- **Customize** — MCP サーバー、プラグイン、スキルなどのコンポーネントを使って、Copilot app に機能を追加します。Playwright MCP の設定に使います。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡単な会話です。最初のチャットを開始すると表示されます。表示されていない場合は、**Sessions** の横の **+** を選択してチャットを始めます。
- **Sessions** — リポジトリフォルダーと、その中のエージェントセッションをまとめます。リポジトリ名 (例: `tailspin-toys`) を選択してリポジトリビューを開くか、その下のセッションを選択して会話に戻ります。各セッションは分離された独自のワークスペースで動くため、変更を競合させずに複数を同時実行できます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意された、現在のバックログを確認しましょう。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選択します。リポジトリの下にある個別のセッションではありません。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号で決めつけず、タイトルを使って次の issue を探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。小規模な最初の変更を完了した後、フィルター機能の Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の項目一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を表示するには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には **quick chat** が適しています。Quick chats ではブランチや worktree を作成せずに質問やブレインストーミングができるため、セッションを必要としない、その場限りの簡単な質問に最適です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。チャットを開始すると **Chats** が表示されます。
2. アプリのセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で実行されるため、変更が競合することなく複数のエージェントを並列実行できることがわかります。会話はいつでも継続でき、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。学習した内容は次のとおりです。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットを使って、その場限りの簡単な質問をする。

次は、最初のエージェントセッションを開始し、ゲームカードに星評価を表示する最初の変更をプロジェクトに加えます。[レッスン 2「星評価の追加で小さな成果を得る」][next-lesson]に進んでください。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の概要][getting-started]
- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app