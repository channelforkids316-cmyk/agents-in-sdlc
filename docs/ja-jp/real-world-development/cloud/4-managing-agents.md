---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視できることも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。この mission control では、pull request (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の内容を学習します。

- エージェントページを使って cloud agent のタスクを監視します。
- 進行中のセッションに指示を送り、機能を追加します。

## シナリオ

エージェントにハイコントラストモードを作成させた後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているので、ライトモードも含めるのが自然です。エージェントの作業に指示を追加し、ハイコントラストモードに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスクの一覧を確認します。次のようなタスクがあります。
    - コードベースのドキュメント更新。
    - ゲーム変更用 API の生成。
    - Web サイトのハイコントラストモード追加。
3. 実行中のタスクを選択し、Copilot が実行した手順を確認します。次のような操作が含まれる場合があります。
    - リポジトリのチェックアウト。
    - Copilot が作業する環境の作成。
    - MCP サーバーの設定。
    - タスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。PR と変更されたファイルを確認し、さらに監視できます。

## Cloud agent に指示を送る

現在実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加するセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されるアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を始めるまで数分間セッションを確認します。次のようなメッセージが表示されたら、作業を開始しています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページで、Copilot が作業中のセッションに指示を送るダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい要望を作業に取り込みます。

前と同じように完了まで数分かかります。その間に、これまでに学習した内容を振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視し、方向付ける方法を学びました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクを変更するよう指示できます。

次の内容を学習しました。

- mission control と agents ページを使って cloud agent のタスクを監視する方法。
- 進行中のセッションに新しい機能の追加を依頼する方法。

Copilot がアクセシビリティ機能の作業を完了したら、次は [Copilot が作成した pull request を改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
