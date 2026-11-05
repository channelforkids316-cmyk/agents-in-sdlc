---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、この機能を組み合わせたカスタムエージェントを作成し、明確な QA プロセスを定義します。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルター機能の issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義される、専門的な Copilot です。エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために Copilot がこれらの機能をどう使うかを示します。いずれも、別の会話を始める必要はありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成させます。プロファイルでは、QA が実施するチェックと、その作業の範囲を定めます。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が想定した QA ワークフローと作業範囲を設定できていることを確認します。これにより、確認だけを依頼したのにプロファイルが不完全だったり、広い範囲でコードを変更したりするのを防げます。

1. `/diff` を実行して `.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は任意ですが、エージェントの表示名を明確にできます。
3. プロファイルの指示を読み、要件の確認、リポジトリ指示の適用、`quality-checks` スキルの実行、Playwright MCP の使用が含まれることを確認します。
4. QA が証拠を報告し、実装コードの変更前に確認を求め、変更をコミットせず PR も開かないことを確認します。
5. これらの責任や制約が欠けている場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクト内のエージェントを読み込みます。プロファイルを作成した後、同じフィルター機能の会話を再開し、QA を選択します。そうすると、すでにコンテキストに含まれる issue と計画の決定を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その作業をレビューしました。このレッスンでは、次の内容を行いました。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認しました。
- 再利用可能な品質保証プロファイルを作成して調べました。
- QA エージェントを選択し、フィルター機能の issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、これらを機能 PR にまとめ、Agent Merge を使います。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
