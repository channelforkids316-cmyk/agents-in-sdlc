---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた専門 AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象を絞った指示、ベストプラクティス、コーディングパターン、専門知識を指定します。チームの知識を再利用できるエージェントにできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを実践するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントと agent skills の違い

カスタムエージェントと [agent skills][agent-skills-concept] はどちらも、主に Markdown ファイルで定義して AI に作業方法を伝えます。わかりやすく区別するなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを編成して作業できます。この演習では、アクセシビリティ用カスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、pull request ワークフローやテストの管理に使うスキルを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。ダークテーマはコントラストが不十分で、読みにくいというフィードバックが寄せられました。デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

この演習では、次の内容を学習します。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。内容を確認して、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、アクセシビリティ作業用に `main` から新しいブランチを作成します (演習 3 のフィルター機能 PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、アクセシブルな Web サイト向けコード生成の責任、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code では、Copilot Chat ビュー下部のエージェント一覧に `.github/agents` 内で定義されたカスタムエージェントが表示されます。カスタムエージェントを選ぶと、そのエージェントの指示とツールに合わせてチャットセッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は手順の中で説明します。

1. Chat ビューのエージェント一覧から **Agent** を選択します (まだ選択されていない場合)。
2. Chat ビュー下部のエージェント一覧を開きます (既定では **default** が表示されています)。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトのファイルに適用します。各編集はファイルパスと差分とともにチャットに表示されます。
6. エージェントが編集したファイルに応じて、**Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に編集します。

次の演習では、進行中の作業を確認し、エージェントに追加の指示を送ります。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野向けの専門 AI アシスタントです。チームの知識と標準を再利用できる形にして、Copilot が作業を進められるようにします。

このレッスンでは、次の内容を学習しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は、[エージェントの作業を監視して方向付けます][next-lesson]。変更内容を確認し、同じセッションでライトモードを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [Awesome Copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
