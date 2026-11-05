---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認してドキュメント標準を追加し、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、希望するコードの内容だけでなく、構成方法も説明できます。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示やパスを対象とする指示ファイルがエージェントにどう伝わるかを確認します。
- コーディング標準を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- コメントでは、コードの内容を繰り返さず、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載します。`db` を引数として注入できる場合は、その説明も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを進められるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードを生成できるようにします。コーディング規約、ライブラリ、好みのコメント形式などを指定できます。リポジトリ全体、または特定の種類のファイル向けに指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。大半のリクエストに関係するプロジェクトレベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクを対象にしたガイドラインです。

> [!NOTE]
> ほかの形式の指示ファイルや、そのサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、いくつかの指示ファイルが用意されています。変更を加える前に内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます (ターミナルではありません)。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべての対話に適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob は指示を適用するファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、参考例を指定できます。

## チームのガイダンスに合わせて指示ファイルを更新する

用意されたファイルは良い出発点ですが、TypeScript にドキュメントコメントを追加するよう `copilot-instructions.md` に指定しましょう。

1. フィルター機能の会話が開いている場合は `/exit` で終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を探します。
4. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。Copilot CLI は会話開始時にリポジトリの指示を読み込みます。更新した指示を使い、機能のコンテキストを保ったまま、保存済みの会話を終了して再開します。

1. まだ開いている場合は `/exit` を入力します。
2. リポジトリのルートにある `game-filters-cli` ブランチから会話を再開します。
3. Copilot に次のプロンプトを送信します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの内容を正しく説明していることを確認します。

## 更新したガイダンスを使う

新しいリポジトリ標準に沿って、Copilot に生成したコードを更新させました。具体的には、次の内容を行いました。

- リポジトリの指示ファイルと、パスを対象とする指示ファイルがエージェントにどう伝わるかを確認しました。
- コーディング標準を守るため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどを集めたリポジトリ][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
