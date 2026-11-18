---
title: "演習 1 - Custom instructions（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成に関するガイダンスを伝えられます。

この演習では、次のことを行います。

- repository custom instructions と path-scoped instruction file を通じて、プロジェクト固有のコンテキスト、coding guideline、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- *現在の* instructions を適用して、フィルタリングに必要な最初の data slice（publisher helper）を生成します。
- `.github/copilot-instructions.md` に repository 全体に適用する標準を追加します。
- 同じ prompt を再実行し、生成コードに新しい標準が反映されることを確認します。
- instructions の更新と filtering slice を `main` に commit し、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot は決定論的ではありません。この演習では、文字単位で同じ出力を得るのではなく、instructions の更新後に動作がどのように変化するかを確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。内容は次のとおりです。

- data layer には常に unit test が必要です。
- UI は dark mode にし、現代的な見た目にします。
- code には TSDoc doc comment 形式でドキュメントを追加します。
- 各 file の先頭に、その file の役割を説明する comment block を追加します。

instruction file を使って Copilot に必要な情報を提供し、このガイドラインに沿ってタスクを進めます。

### Custom instructions

custom instructions を使うと、Copilot が coding style と要件を理解できるよう、コンテキストと好みを伝えられます。coding convention、library、使いたい comment の種類などを指定することで、より関連性の高い提案や code snippet を得られます。repository 全体や、特定の種類の file に適用する instructions を作成できます。

instruction file は 2 種類あります。

- `.github/copilot-instructions.md` は、repository に対する**すべての** request で Copilot に送信される単一の instruction file です。この file には、使用技術、開発中のものの概要、ベスト プラクティスなど、多くの chat や CLI request に関係する project-level の情報を記載します。
- `.github/instructions/*.instructions.md` file は、特定の task や file type 向けに作成できます。TypeScript や Astro などの言語、UI component や unit test の作成などの task に関するガイドラインを記載できます。

> [!NOTE]
> IDE では、instruction file は Copilot Chat の code generation にのみ使われ、code completion や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、repository-level instructions と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使ってコードを生成します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準で記述された instruction file にも対応しています。[instruction file のサポート][custom-instructions-support]を参照してください。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成について詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトの例は一般的な方法を示しています。概要は次のとおりです。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体に適用する coding standard など、project-level のガイダンスを記載します。
- `*.instructions.md` file には、unit test、Astro component、data layer など、file type や特定の task に関する instructions を記載します。
- 自然な言葉で、明確なガイダンスを記載します。コードをどのように書くべきか、また、どのように書くべきでないかを例で示します。

instruction file の作成方法は 1 つではありません。AI のほかの使い方と同様に、試しながらプロジェクトに最も適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトで、十分な instruction file を用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな task 向けの file が含まれています。
>
> Copilot に instruction file を生成させることもできます。操作方法は利用する surface によって異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。該当する surface のレッスンで、必要な操作を説明します。
>
> template や出発点を探している場合は、instruction file、custom agent、その他の resource を集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instructions file を確認する

この repository には、中心となる `copilot-instructions.md` と、さまざまな task 向けの `*.instructions.md` file が用意されています。editor または GitHub web UI で開き、内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などの section を確認します。**Code standards** の下にある、入れ子になった **GitHub Actions Workflows** のガイダンスにも注目します。これらは、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` folder を開いて内容を確認します。Astro file、Drizzle data layer、test などの instructions が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instructions が適用される file を決める glob を指定します。この例では、`**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトで unit test を作成するための固有の instructions を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などのほかの instruction file や、プロジェクト内の file への link に注目します。これにより、より大きな instructions を小さな再利用可能 file に分け、Copilot が参考にできる例を提示できます（path は repository root ではなく、instruction file からの相対 path です）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** section にはプロジェクトの coding standard が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc doc comment と file comment header の rule を追加します。

## 変更用 branch を作成する

ここではベスト プラクティスに従い、作業用の branch を作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しい terminal を開きます。
3. branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instructions を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model の選択は演習ごとに異なるため、該当する手順で説明します。

custom instructions の効果を確認するため、最初に現在の instruction file を使って prompt を送信します。その後 file を更新し、同じ prompt を再送信して違いを確認します。

1. 前の演習で開いた editor tab をすべて閉じ、Copilot が必要な context のみを読み込むようにします。
2. `src/lib/publishers.ts` を開き、helper の配置先を Copilot に伝えます。
3. Chat view の agents dropdown で **Agent** を選択し、Copilot が file の変更を適用できるようにします。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認し、必要なコードを変更します。helper file と test file の両方を変更する場合もあります。
6. 提案された helper は、最初の引数に `db` client を受け取り、型付きの publisher 配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の data-layer 慣例によるものです。
7. 提案されたコードに TSDoc doc comment と file-level comment header が**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。instructions 更新後の動作が一貫することが、この演習の目的です。

## repository 全体に適用する標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は project-level の情報を Copilot に提供します。repository の coding standard を記載し、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** section を探します。およそ 27 行目にあります。project の coding standard はありますが、コード内のドキュメントに関する rule はありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。file comment header と TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、repository-level の `.github/copilot-instructions.md` には全体に適用するガイダンスを、`*.instructions.md` file には特定の言語、file type、task に関するガイダンスを記載します。今回追加した doc comment rule のような project-wide の標準には、repository-level file が適しています。

## prompt を再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選び、Copilot に対象 file を認識させます。
3. 前と**同じ prompt**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案された file の先頭に、次の例のような comment block があることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次の例のような TSDoc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を誘導して新しい project standard に従わせ、次の演習で使う実際のコードに適用できました。

## instructions を commit して branch を push する

instruction file は repository 内のほかの asset と同様に、source control で管理します。instructions と helper の変更を commit し、branch を push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しい terminal window を開きます。
2. terminal で `git status` を実行し、instructions と helper の変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. instructions の更新と helper の基盤として Copilot が変更した file を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch を repository に push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

branch を push したので、GitHub Copilot を使って pull request を作成し、backlog のドキュメント標準に関する issue と関連付けます。GitHub tools はすでにプロジェクトに接続されているため、Copilot に issue を探し、PR を作成して issue を閉じ、マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。project template では GitHub tools が workspace に設定済みです。初めて使うときに VS Code で GitHub sign-in を求められる場合があります。案内に従って認証してください。この接続方式である Model Context Protocol については、後の演習で説明します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat view を開き、agent picker で **Agent** が選択されていることを確認します。
3. instructions file の更新と、次の filtering 作業に向けた publishers helper の追加を説明する PR を、現在の branch から作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が GitHub 操作の実行許可を求めたら、command を確認し、適切な場合に **Allow** を選択します。
5. PR が作成されたら、次の prompt で PR をマージしてローカル branch を `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作や shell command の実行許可を求めたら、command を確認し、適切な場合に **Allow** を選択します。

GitHub Copilot を使って pull request を作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの instruction file からコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の instructions を使って code-generation prompt を送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に repository 全体に適用する標準を追加しました。
- 同じ prompt を再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で使う instructions と helper の基盤を `main` に commit しました。

次は [Agent mode][next-lesson]を使って、Copilot に codebase 全体の新しい機能を追加してもらいます。

## リソース

- [GitHub Copilot の custom instructions file][instruction-files]
- [Custom instructions を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instructions を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instructions][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
