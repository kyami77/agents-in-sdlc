---
title: "演習 1 - Custom instructions（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用する際は、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[Instruction file][instruction-files] を使うと、実現したいことだけでなく、その方法も Copilot に伝えられます。

この演習では、次のことを行います。

- repository custom instructions と path-scoped instruction file を使って、プロジェクト固有のコンテキスト、coding guideline、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` に repository 全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI の harness とは異なり、ここでは prompt の実行前後を比較しません。Copilot cloud agent は GitHub issue を使って非同期に作業するため、リアルタイムで並べて効果を示すのが難しいためです。この harness の後半で cloud agent が作成する pull request を確認する際に、追加した instruction file の影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。

- data layer には常に unit test が必要です。
- UI は dark mode にし、現代的な見た目にします。
- code には TSDoc doc comment 形式でドキュメントを追加します。
- 各 file の先頭に、file の役割を説明する comment block を追加します。

instruction file を使って Copilot に必要な情報を提供し、これらの開発手法に沿ってタスクを進めてもらいます。

### Custom instructions

custom instructions を使うと、Copilot が coding style と要件を理解できるように、コンテキストや好みを伝えられます。coding convention、library、好みの comment 形式を指定することで、より関連性の高い提案や code snippet を得られます。repository 全体、または特定の種類の file に適用する instruction を作成できます。

instruction file は 2 種類あります。

- `.github/copilot-instructions.md` は、repository に対する**すべての** request で Copilot に渡される単一の instruction file です。Copilot Chat や CLI で送る多くの request に関連する、project-level の情報を記載します。たとえば、利用している tech stack、開発中のものの概要、ベスト プラクティス、全体に適用するガイダンスなどです。
- `.github/instructions/*.instructions.md` は、特定のタスクや file type に応じた instruction を作成するためのものです。TypeScript や Astro などの言語、UI component や新しい unit test の作成などのタスクに関するガイドラインを記述できます。

> [!NOTE]
> IDE で作業するとき、instruction file は Copilot Chat の code generation にのみ使われます。code completion や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、repository-level instruction と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使ってコードを生成します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準で記述された instruction file にも対応しています。[instruction file のサポート][custom-instructions-support]を参照してください。

### Instruction file を管理するためのベスト プラクティス

instruction file の作成に関する詳細な説明は、このワークショップの範囲外です。ただし、sample project に含まれる例は、一般的な方針を示しています。

- `copilot-instructions.md` には、開発中のもの、プロジェクトの構造、全体に適用する coding standard など、project-level のガイダンスを記載します。
- `*.instructions.md` には、unit test、Astro component、data layer など、file type や特定のタスクに関する instruction を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。コードをどのように書くべきか、また、どのように書くべきではないかの例も含めます。

instruction file に唯一の正解の書き方はありません。AI の使い方と同様に、試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な種類の instruction file を用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けの file が含まれています。
>
> Copilot を使って instruction file を生成することもできます。操作方法は利用する surface によって異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。該当する surface のレッスンで、必要な操作を説明します。
>
> template や出発点を探している場合は、instruction file、custom agent、その他の resource を集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instructions file を確認する

repository には、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` file が用意されています。editor または GitHub の web UI で開いて内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などの section を確認します。**Code standards** の下には **GitHub Actions Workflows** の詳細なガイダンスもあります。これらは、Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` folder を開いて内容を確認します。Astro file、Drizzle data layer、test などの instruction が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` field は、instruction が適用される file を決める glob（repository root からの相対 path）です。この場合、`**/*.test.ts` に一致する TypeScript test file が対象です。
5. このプロジェクトの unit test 作成に関する instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などのほかの instruction file や、プロジェクト内の file への link があります。これにより、より大きな instruction を小さな再利用可能 file に分け、コード生成の参考となる例を Copilot に示せます（path は repository root ではなく、instruction file からの相対 path です）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** section にはプロジェクトの coding standard が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc doc comment と file comment header のルールを追加します。

## repository 全体に適用する標準を追加する

次は、この演習で行う唯一の編集です。コード内のドキュメントを TSDoc doc comment と file-level comment header で記述する、project-wide rule を追加します。後で issue を割り当てると、cloud agent はこの rule を参照します。

編集の前に、作業用の branch を作成します。cloud agent は issue の対象 branch から instruction を読み込みますが、この変更を独立して commit します。

1. Codespace terminal で、新しい branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は project-level の情報を Copilot に提供します。repository の coding standard を記載して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. **Code formatting requirements** section を探します。およそ 27 行目にあります。project の coding standard が記載されていますが、コード内のドキュメントに関する rule はありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。file comment header と TSDoc doc comment の追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、repository-level の `.github/copilot-instructions.md` には全体に適用するガイダンスを、`*.instructions.md` file には特定の言語、file type、タスクに関するガイダンスを記載します。今回追加した doc comment rule のような project-wide standard には、repository-level file が適しています。

## instructions の更新を commit、push、マージする

cloud agent は、issue の対象 branch にある instruction file を読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` から branch を作成します。そのため、cloud agent が変更を使えるように、この instruction を `main` にマージする必要があります。

1. stage と commit を実行します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. branch を push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に pull request を作成し、マージします。次の演習で issue を割り当てると、cloud agent がこの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、branch の作成を省略して、`main` に直接 commit できます。ここでは、実際のプロジェクトの workflow を反映するために branch を使います。

## まとめと次のステップ

このプロジェクトで Copilot が instruction file からコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に repository 全体の標準を追加しました。次の演習以降、cloud agent が作成する pull request で、この標準が反映される様子を確認します。

次は、Copilot cloud agent に[最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instructions file][instruction-files]
- [Custom instructions を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の custom instructions を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
