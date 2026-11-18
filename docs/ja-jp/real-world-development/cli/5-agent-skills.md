---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、報告形式をカスタマイズして、フィルタリングを検証するために使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、instruction file を使って標準に従わせました。では、test や lint、継続的インテグレーション（CI）のほかの作業はどうでしょうか。

このような作業には **agent skill** が適しています。skill を使うと、test などの操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の報告形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys は unit test、lint、type check に `quality-checks` skill を使っています。チームでは、結果を読みやすくするため、報告を改善したいと考えています。

## Instructions、script、resource

Agent skill は再利用可能なタスク instructions、実行可能な script、補助 resource をまとめたものです。skill の基本は、skill 名を持つ folder と、その中の `SKILL.md` file です。Markdown には、skill の名前と説明を定義する frontmatter、skill の概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、呼び出された skill が使う script やその他の resource をサブフォルダーに含められます。

> [!NOTE]
> skill に追加 folder や file は必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクト内にある既存 command を使うため、`SKILL.md` のみで構成されています。

skill はプロジェクトの `.github/skills` folder に配置すると repository の共有 asset としてチームで再利用できます。または、ユーザーの skill folder `~/.copilot/skills` に配置できます。

## skill を確認する

unit test、lint、type check の実行用に Tailspin Toys チームが作成した `quality-checks` skill を確認しましょう。

1. Codespace に戻ります。Codespaces editor で `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。description によって、Copilot は skill を呼び出すタイミングを判断します。
3. instructions を読み、test と lint の処理をどのように進めるかを確認します。
4. skill にはまだ **Results output formatting** section がないことを確認します。

## 変更を加える前に skill を実行する

Copilot CLI から直接 skill を実行することも、自然な言葉で依頼することもできます。3 種類の check を実行するよう Copilot に依頼します。

1. Interactive mode のフィルタリング会話に戻ります。
2. 次の prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示される報告を確認します。

## 報告をカスタマイズする

どの check を実行したか、成功したか、tool が実際に報告した内容を確認できる報告に改善しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次の section を file の末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. file は自動的に保存されます。

## 更新した skill を実行する

更新した skill を実際に使ってみましょう。Copilot CLI では、会話を再起動せずに編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じ prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示された報告を確認し、最初の報告と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- unit test、lint、type check 用の `quality-checks` skill を確認しました。
- 結果の報告形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は filtering とともに feature PR に含めます。次は、[Playwright MCP server][next-lesson]を使って、Copilot がサイトを直接操作できるようにします。

## ほかの skill の例

次の community example は参考資料です。追加の作業ではありません。

- [Agent Skills specification][skill-spec]
- [Contribution workflow: `make-repo-contribution`][contribution-example]
- [Requirements document: `prd`][prd-example]
- [Diagram と export script: `drawio`][drawio-example]
- [Browser test: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
