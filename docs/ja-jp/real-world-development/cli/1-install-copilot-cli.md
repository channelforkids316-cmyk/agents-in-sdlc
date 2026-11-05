---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、プロジェクトを確認して、用意されたフィルター機能の issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを使えます。タスクを任せたり、変更を依頼したりできるため、コマンドラインから作業に集中できます。まずツールをインストールしましょう。使い慣れたツールで簡単にインストールできます。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を探します。

## シナリオ

チームでは、増え続けるバックログに対応するため AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでエージェントの機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して「はい」と回答します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されていることがあります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 操作に慣れる

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動した後は、自然言語をエージェントに送り、slash command で会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選択し、<kbd>Enter</kbd> を押して、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールしたバージョンのコマンド一覧を確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が動作することを確かめるため、簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べていることを確認します。
5. `/mcp list` を入力して、組み込みの GitHub MCP サーバーを利用できることを確認します。
6. Copilot にフィルター機能の issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の小さな変更を終えた後、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的に worktree は作成されません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、通常、設定済みの権限の範囲外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を使って再起動します。ユーザーが許可したこのショートカットにより、Codespace 内では承認のプロンプトが省略され、演習に集中できます。

> [!CAUTION]
> `--yolo` を使うと、すべてのツール、パス、URL に対する自動許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) が有効になります。Codespace や VM などの分離された環境でのみ使い、日常の開発で既定値として alias に設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、後のレッスンで issue や pull request を操作する際に使う、読み取りと書き込みの GitHub MCP ツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりしたら、`copilot --resume` を使って同じ機能会話とブランチに戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証しました。このレッスンでは、次の内容を行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけました。

次は[最初の小さな変更に取り組み][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
