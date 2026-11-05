---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。まとめてレビューし、pull request を作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルター機能と品質ワークフローの作業を 1 つの会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証の証拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後に Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げているもの (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合の rebase など) を処理します。その後、リポジトリの設定が許可すれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューし、マージできる状態だと判断してください。

## Agent Merge で PR を管理する

コードの作成が完了しました。すべてをまとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を実行して、エージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行し、新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は報告内容を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、pull request の作成など、開発プロセスの一部を自動化しました。このレッスンでは、次の内容を行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意の cloud agent への委任に役立つ [Copilot CLI の slash command を確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
