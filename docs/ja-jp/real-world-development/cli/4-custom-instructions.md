---
title: "レッスン 4 - custom instructions で Copilot を導く"
description: "リポジトリの instructions を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う場合、コンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が利用できるようにします。[Instruction file][instruction-files] では、求めるコードの内容だけでなく、コードの構成方法も説明できます。フィルタリング機能を構築したので、Copilot が参照した instructions を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの instructions と、path-scoped instruction file が agent にどのように渡されるかを確認します。
- coding standard が守られるように instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。内容は次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数では、`db` 引数を注入できる場合はそれも含め、目的、parameter、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro component では `Props` contract を説明し、関連するコードの変更後もコメントを最新の状態に保ちます。
- 既存の formatting と lint のガイダンスを維持します。

instruction file を使って Copilot に必要な情報を提供し、このガイドラインに沿ってタスクを進めてもらいます。

## Instruction file

custom instructions を使うと、Copilot が coding style と要件を理解できるように、コンテキストや好みを伝えられます。coding convention、library、好みのコメントなどを指定することで、より関連性の高い提案や code snippet を得られます。リポジトリ全体、または特定の種類のファイルに適用する instruction を作成できます。

instruction file は 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての** request で Copilot に渡される単一の instruction file です。ほとんどの Copilot request に関連するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、file type、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの instruction 形式のサポート状況は、環境によって異なります。特定の形式に依存する前に、[custom instructions のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instructions file を確認する

starter project には、すでに複数の instruction file が含まれています。変更を加える前に、内容を確認して効果を把握しましょう。

1. Codespace に戻ります。
2. Codespaces editor で（terminal ではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明と coding guidance を確認します。この instruction は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` folder を開き、ファイルを確認します。Astro file、Drizzle data layer、test などの instructions が含まれています。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` field を確認します。この field は、instruction が適用されるファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへの link を確認します。これらを使うと、instruction を小さな再利用可能ファイルに分け、Copilot が参照する例を示せます。

## チームのガイダンスに合わせて instructions file を更新する

既存の file はよい出発点ですが、追加で対応したい点があります。中心となる `copilot-instructions.md` file を変更し、新しく生成される TypeScript に TSDoc comment が含まれるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** という見出しの section を探します。およそ 35 行目にあります。
2. 最後の bullet として、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

file は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instructions を読み込みます。instruction を保存した後、feature のコンテキストを保ったまま Copilot CLI を終了して、フィルタリングの会話を再開します。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリの root にある `game-filters-cli` branch から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   prompt が表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` branch が checkout されていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript file を確認します。新しい TSDoc comment が追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。次のことを行いました。

- リポジトリの instructions と、path-scoped instruction file が agent にどのように渡されるかを確認しました。
- coding standard が守られるように instruction file を更新しました。
- instruction file がコードに与える影響を確認しました。

次は、再利用可能な quality-checks skill を[カスタマイズして実行し][next-lesson]、lint と test を一貫して実行できるようにします。

## リソース

- [Copilot CLI に custom instructions を追加する][instruction-files]
- [Custom instructions のサポート][custom-instructions-support]
- [Custom instructions を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
