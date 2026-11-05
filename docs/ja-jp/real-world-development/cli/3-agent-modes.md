---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で issue からフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。大きな変更には、より堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の流れに合わせて作業できるため、適切な方法で必要な機能を作成できます。このレッスンから数回にわたり、一般的なエージェント主導の開発プロセスに沿って進めます。まず issue を使って機能を作成し、コードが有効で、機能が想定どおりに動作することを確かめてから、正常にマージします。

> [!NOTE]
> 機能ワークフローでは、同じ会話とブランチを引き続き使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能の作成を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの issue には機能が説明されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておきたい詳細があります。Plan モードで決定し、Autopilot に範囲を限定した実装を任せます。

## 背景

AI コーディングエージェントを開発フローに加えても、基本的な工程は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 実装する内容を説明した issue から始めます。
2. 作成するものを計画します。
3. コードを作成してレビューします。
4. テストを実行し、コードを検証します。
5. 新しい機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れに近いものです。

この標準的な進め方に従うと、AI が生成したコードが要件を満たし、手作業で作成したコードと同じレビューを受けていることを確認できます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードを作成し、テストを実行して、タスクが完了するまで自律的に改善します。

最初に Plan モードで計画をレビューしてから、Autopilot で実装します。

## issue から始める

フィルター機能に取り組む前に、Codespace とリポジトリの準備を確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の issue や pull request (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、Copilot に機能実装やタスクを任せる前に、進め方を決められます。複雑な作業では、時間をかけて計画を立てると役立ちます。Plan モードに切り替え、Copilot に計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら <kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読みます。
6. 自分の決定に合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が完成し、レビューできたら、Autopilot モードで Copilot に実装を依頼します。Autopilot は、タスクが完了したと判断するまで Copilot が問題に取り組むことを可能にします。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画をレビューしてください。依存関係が見つからない場合やポートが競合する場合は、セットアップの問題を解決してからチェック結果を完了と見なしてください。

## 実装をレビューして検証する

コードの生成後、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** が出ていないことを確認します。
2. `/diff` を実行し、フィルターの実装とテストを確認します。
3. 結果を issue と計画で決めた内容に照らして確認します。
4. レビュー後、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開いて動作を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行元のフィルターや組み合わせを試します。
4. リセットと結果が空になる場合の動作が、issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を作成し、レビューしました。このレッスンでは、次の内容を行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、コード生成の動作をさらに詳しく確認し、記載された手法に従うよう [カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
