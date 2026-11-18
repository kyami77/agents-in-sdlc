---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、UX の改善と機能の追加を目的に、GitHub Copilot といくつかの task に取り組んできました。Copilot にコードのドキュメント追加、design team 向けの関連ゲーム機能の実装、high-contrast mode と light-mode toggle を含む accessibility 機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot に feedback を送って作業を改善する方法を確認しましょう。

### シナリオ

生成 AI を導入しても、software design と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps process を進めます。チームメンバーがレビューできるようにする前に、ドキュメント、関連ゲーム機能、accessibility 機能について GitHub Copilot が提案した変更を確認しましょう。

## Security と GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期に作業するため、安全を保つためにいくつかのセキュリティ制約が設けられています。

- Copilot は repository への読み取り権限を持ち、書き込み権限は作業に使う branch **のみ**に持ちます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- MCP server を含め、[外部 resource へのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent がコードにドキュメントを追加した pull request（PR）を確認します。GitHub.com の標準的な PR interface を使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot が firewall によりブロックされたという warning が表示される場合があります。Copilot は、外部 MCP server への call など外部 resource へのアクセスが既定で制限されているため、**想定された動作です**。必要に応じて、[Copilot cloud agent の firewall をカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の repository に戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様の title が付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** flag が表示されます。作業が完了するまで待ってください。数分かかることがあります。休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** tab を選択し、変更を確認します。

    ![Files changed tab](../../../_images/shared-pr-files-changed.png)

5. TSDoc doc comment など、新しく追加されたドキュメントを確認します。実際の変更は異なる場合があります。

   変更を確認するときは、TSDoc doc comment、TypeScript の慣例、comment header を探します。これらは、演習 2 の最初に確認した custom instruction file によるものです。

6. 更新内容を確認して問題がなければ **Conversation** tab に戻り、下にスクロールします。
7. 一部の workflow が承認待ちになっていることを示す表示を探します。
8. workflow が承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks section で workflow が queue に追加されたことを確認します。問題がなければ、単一 Astro app の project check が成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業する際、やりとりは一方通行ではありません。チームメンバーと同様に、pull request の comment やコードへの inline comment で Copilot に知らせることができます。Copilot は comment を確認し、対応する新しい session を開始します。結果は予測できないため、必ず依頼すべき決まった文面はありません。たとえば、次のような変更を依頼できます。

- 各コード file の先頭に、file の簡単な説明を comment header として追加する。
- TypeScript と Astro file に TSDoc doc comment を追加する。
- Astro app の構造を説明する README を作成する。

1. 上記の例、または codebase に追加したいドキュメントについての別の提案を使い、**@copilot** を指定して生成されたドキュメントへの変更を依頼する comment を追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新のために新しい session を開始します。
3. **Back to pull request** を選択して、pull request に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、pull request に新しい commit が追加されます。
5. **Files changed** tab を選択し、変更を確認します。

満足するまで作業を繰り返せます。問題がなければ、PR を draft から ready に変更し、`main` branch にマージできます。

![PR を review 用に ready にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue について、Copilot が作成した PR を確認します。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** tab を選択します。
3. **Show related games on the game details page** または同様の title が付いた PR を選択します。
4. **Files changed** tab を選択し、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ **Conversation** tab に戻り、下にスクロールします。
6. 一部の workflow が承認待ちになっていることを示す表示を探します。
7. workflow が承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request の checks section で workflow が queue に追加されたことを確認します。問題がなければ、単一 Astro app の project check が成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこの branch に切り替えて、関連ゲーム機能を手動で試せます。Codespace に移動し、terminal を開いて次の command を実行します（`<branch-name>` を Copilot が作成した branch 名、例: **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に追加の変更を依頼できます。たとえば、表示する関連ゲームの数を調整する、comment header と TSDoc doc comment を追加するなどです。この task は、custom instructions を更新する**前**に割り当てたことを思い出してください。前と同じように **Conversation** tab に comment を追加すると、Copilot が確認して新しい session を開始します。

## Accessibility 機能をレビューする

最後に、accessibility custom agent を使って実装した accessibility 機能を確認します。この PR には、演習 3 で依頼した high-contrast mode と、演習 4 の mission control で追加を依頼した light mode の両方が含まれます。

1. GitHub.com の repository に戻ります。
2. **Pull Requests** tab を選択します。
3. **Add high contrast mode to website** または同様の title が付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request に **[WIP]** flag が表示されます。作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** tab を選択し、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - mode を切り替える UI component。
   - 利用者の設定を保持する local storage の使用。
   - high-contrast と light mode の CSS または style の変更。
   - accessibility attribute（ARIA label、keyboard navigation など）。
   - mode の切り替えを管理する JavaScript/TypeScript code。
6. 更新内容を確認して問題がなければ **Conversation** tab に戻り、下にスクロールします。
7. 一部の workflow が承認待ちになっていることを示す表示を探します。
8. workflow が承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks section で workflow が queue に追加されたことを確認します。問題がなければ、単一 Astro app の project check が成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこの branch に切り替えて、accessibility 機能を手動で試せます。Codespace に移動し、terminal を開いて次の command を実行します（`<branch-name>` を Copilot が作成した branch 名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、app を起動して browser で high-contrast と light-mode toggle を試し、ページを再読み込みしても設定が保持されることを確認します。

accessibility custom agent が accessibility のベスト プラクティスに沿うよう Copilot を導いたことを確認してください。accessibility に関する問題や改善点があれば、前の PR と同様に comment で **@copilot** を指定して修正を依頼できます。

## 任意の演習 — さらに task を委任する

cloud agent は実際の backlog item を依頼し、その間に別の作業を進める場合に役立ちます。習慣を身につけるために、repository に issue を追加して Copilot に割り当ててみてください。次のような例があります。

- game details page に、支援者が関心を示すためのフォームを作成する。
- game list page に pagination を実装する。
- data-access helper に入力検証と error handling を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンを通じて、次のことを行いました。

- **repository に含まれる custom instruction file を確認し**、cloud agent の出力に反映される様子を後で確認できるようにしました。
- **issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- accessibility 用の custom agent を作成して使い、high-contrast と light-mode toggle を追加しました。
- **Copilot agents page を mission control として使い**、accessibility session を監視して方向を示しました。
- `@copilot` に変更を依頼し、workflow を承認して、cloud agent の pull request をレビューし反復しました。

## レビューと次のステップ

Cloud agent harness を完了しました。さらに確認する場合は、ほかの harness で今回の内容を広げられます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE から Copilot Chat agent mode と MCP integration を確認します。
- 💻 **[CLI harness](../../cli/)** — terminal から Plan と Autopilot mode、agent skill、custom QA agent、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、レビュー済みの変更を 2 つ届けます。

自分の repository では、次の案を試してみてください。

- refactoring または test coverage の issue を cloud agent に割り当てる。
- 別の分野の custom agent を作成する。
- 別の tech stack 用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall を構成する][agent-firewall]

---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
