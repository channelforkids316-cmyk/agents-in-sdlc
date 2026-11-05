---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキストの管理、モデルの選択、セッションの共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使える slash command が多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で使用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを試します。

## シナリオ

CLI の主なワークフローを終えました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有など、追加機能を確認します。また、[Copilot cloud agent][about-cloud-agent] に作業を委任することもできます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキストウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また slash command でコンテキストを確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。まだ Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧表示と選択ができます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を実行し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、たとえばローカルの Markdown ファイルに出力する `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を予定していないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業の拡張と委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに役立つ次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列で実行できます。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送信します。cloud agent は非同期に作業し、pull request を作成する場合があります。

これらのコマンドは、worktree の作成やリモートの作業を伴うため、任意の機能です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定やセッション共有、内部状態の確認ができます。このレッスンでは、次の内容を行いました。

- `/context` と `/compact` でコンテキストの管理方法を確認しました。
- `/model` で使用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への任意の委任に使うコマンドを確認しました。

ほかにも多くの slash command があります。[学んだ内容を振り返り、次のステップを確認して][next-lesson]、Copilot CLI での学習を締めくくりましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
