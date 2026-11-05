---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次の内容を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューし、最初の pull request (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルを調整して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- QA カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge にフィルター機能の PR を任せました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使う slash command を確認しました。

## 作成したもの

ワークショップでは、更新した `main` からそれぞれ独立したブランチを作成し、2 つの PR を作りました。

1. **星評価:** 既存の `starRating` と未評価の場合の明示的な表示をゲームカードに追加しました。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` の報告形式を変更し、QA プロファイルと関連テストを追加しました。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。手順を簡潔にするため、作業を 1 つの PR にまとめました。

## 検証の種類

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果と、要件およびテストカバレッジのレビューをまとめて PR の前に確認しました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行時に追加テストが不要と判断される場合もあります。マージを許可する前にコードと証拠を確認し、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示の更新、スキルの調整、QA プロファイルの作成、MCP サーバーの設定を行いました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を考えるとき、**Interactive** は変更に取り組みながら確認を続けるとき、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には速いモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作成したいのかを明確に伝えると、結果が大きく変わります。

## さらに確認する

ワークショップの主要な内容は完了しました。次の CLI 機能も試せます。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に編成します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることで上達します。プロダクションコード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してください。チームで知見を共有し、互いに学び合いましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
