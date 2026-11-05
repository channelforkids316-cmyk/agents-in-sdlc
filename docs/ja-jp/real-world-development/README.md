---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を繰り返し計画、実装、テストし、pull request をレビューし、エージェントを使って作業を管理します。

## シナリオ

架空の会社 Tailspin Toys で、新しい開発者として働きます。Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングを運営しています。チームのバックログは GitHub issue として作成済みです。機能作業にはフィルターやページネーション、品質改善にはアクセシビリティやコーディング標準などがあります。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発方法に合わせて環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan と Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的な slash command を試します。

### [GitHub Copilot app][app]

デスクトップアプリで並列のエージェントセッションを実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を進めてもらいます。進捗を監視して方向付けし、作成された pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントをエディターから操作します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
