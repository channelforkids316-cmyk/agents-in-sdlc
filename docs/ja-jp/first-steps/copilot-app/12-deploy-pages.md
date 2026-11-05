---
title: "レッスン 12 - GitHub Pages でサイトを公開する"
description: "GitHub Copilot app を使って Pages ワークフローを設定し、pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz を公開し、誰でもアクセスできる場所を作りましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのはあなたの役目です。

このレッスンでは、次の内容を学習します。

- Copilot に GitHub Pages の設定を依頼します。
- マージ前にワークフローと pull request をレビューします。
- デプロイの進行状況を確認し、公開されたサイトを検証します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、まだ誰も簡単には試せません。GitHub Pages で単一ファイルのサイトを公開すれば、サーバーやランタイム依存関係を追加せずに公開 URL からアクセスできるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz の pull request がマージされ、GitHub Actions の Deploy website ワークフローで Build website、Upload artifact、Deploy to Pages の各ステップが成功し、yourname.github.io/space-quiz/ でサイトが公開されるまでの3段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、エージェントに pull request の作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。続行する前に、差分、デプロイ権限、GitHub の承認プロンプトを確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がまだない場合に限り、追加されていることを確認します。
2. pull request の変更内容と、ワークフローのビルドおよびデプロイ手順をレビューします。
3. 必須の pull request チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを実行できない場合は、具体的な指示に従ってください。デプロイが成功し、公開サイトを表示できるまでは、作業が完了したと見なさないでください。

## まとめと次のステップ

Space Quiz が公開され、エージェントの報告だけに頼らず、デプロイを確認できました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
