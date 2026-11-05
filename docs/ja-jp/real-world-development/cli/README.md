---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、pull request 自動化を学びながら、Tailspin Toys に 2 つの変更を実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できます。コードベースの探索、コードの生成、コマンドの実行、外部ツールへの接続が可能です。すべてコマンドラインで行えるため、グラフィカルエディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価を追加する対象を絞った変更を行い、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. QA カスタムエージェントを作成して、要件、カバレッジ、検証の証拠をレビューします。
8. フィルター機能の変更全体をレビューし、Agent Merge を使って PR を管理します。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に役立つ slash command を確認します。

ワークショップの焦点を絞るため、PR は 2 つ作成します。星評価の PR と、指示、スキル、QA プロファイル、テストの更新を含むフィルター機能の PR です。フィルター機能と品質ワークフローの作業では 1 つの会話とブランチを使い、各ツールの使い方を学びながら作業を引き継ぎます。

## レッスン

| レッスン | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証します |
| [2. 星評価の追加で小さな成果を得る][ex2] | 最初の変更 | ゲームカードに評価を表示し、最初の pull request をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Plan で要件を決め、Autopilot でフィルター機能を実装してレビューします |
| [4. カスタム指示による Copilot のガイド][ex4] | コンテキスト | リポジトリの指示を確認、更新し、フィルター機能に適用します |
| [5. Agent Skills による作業の標準化][ex5] | 繰り返し実行できるチェック | `quality-checks` スキルをカスタマイズして実行します |
| [6. Playwright MCP による機能の検証][ex6] | ブラウザーでの検証 | Playwright MCP を追加し、ブラウザーでフィルターを確認します |
| [7. QA エージェントの作成と使用][ex7] | 品質保証 | Issue、スキル、MCP をまとめる QA エージェントを作成します |
| [8. 機能の PR の作成とマージ][ex8] | レビューとマージ | 変更をレビューし、Agent Merge を使います |
| [9. Copilot CLI の便利な slash command][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への委任を確認します |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフローと追加のリソースを確認します |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、Backer Concierge を構築してデプロイし、Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドラインの基本的な知識

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[レッスン 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
