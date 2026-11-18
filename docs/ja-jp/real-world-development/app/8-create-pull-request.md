---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "filtering、instructions、skill の更新、QA profile、test をまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

filtering の実装、instruction の更新、skill の更新、quality assurance（QA）profile、test を1つの branch に保存しました。すべてをまとめてレビューし、pull request を作成します。star-rating の pull request（PR）は自分でマージしましたが、今回は **Agent Merge** にマージのライフサイクルを管理させます。

> [!NOTE]
> 通常は、機能、instruction の更新、skill の更新、QA agent をいくつかの別々の PR に分けます。ワークショップを簡潔に進めるため、filtering と quality workflow 全体を1つの session と branch で行い、その作業をすべてこの PR にまとめています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能の PR 全体と検証結果を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

filtering の workflow 全体を通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** を使うと、GitHub Copilot app で pull request を取り込むために必要な残りの作業を自動化できます。有効にすると、app の session が pull request を読み取り、失敗した continuous integration（CI）check の修正、review comment への対応、必要に応じた rebase など、マージを妨げている問題に対処します。GitHub が許可するとすぐにマージします。Agent Merge はバックグラウンドで実行され、app を再起動しても継続し、pull request がマージされると自動的に停止します。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、コードの編集やマージには、引き続き明示的な許可が必要です。マージ権限を与える前に、許可される操作と作業内容を確認してください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したので、Agent Merge に PR の処理を任せましょう。

1. agent picker で **Default agent** を選択します。
2. **Create PR** の横にある dropdown を選択します。
3. **Agent Merge** を選択します。button の表示が **Agent Merge** に変わります。
4. **Agent Merge** button を選択して、処理を開始します。dropdown の項目を選んだだけでは開始されません。

Agent Merge の処理が始まります。次の作業を行います。

- title と説明を付けて pull request を作成します。
- Issue から session を開始していた場合は、説明本文で関連する Issue を参照します。
- target branch に対して rebase を行うか、マージ競合に対処します。
- CI を監視して、すべての check が成功することを確認します。
- 他の開発者や Copilot code review からの feedback がないか PR を監視し、comment を解決するために更新します。
- すべての check が成功した後、必要に応じて PR を自動的にマージします。

すべての条件が満たされたら、Agent Merge に PR もマージさせましょう。

5. マージを許可する前に、PR の diff と検証結果をレビューします。
6. **Agent Merge** の横にある dropdown を選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. agent に明示的にマージを依頼します。

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 処理の進行状況を確認し、報告された阻害要因に対処します。次に進む前に、PR の status が **Merged** であることを確認してください。マージ権限が有効になっていることや、agent が実行中であることだけでは、マージ完了の確認にはなりません。

> [!IMPORTANT]
> Agent Merge は、repository の保護ルールや権限不足を回避しません。続行する前に、阻害要因を解決してください。

## まとめと次のステップ

コード生成、test と検証、pull request の処理など、開発 process の複数の作業を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能の PR 全体と検証結果を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は、[既存の canvas を使って triage canvas を作成し][next-lesson]、agent と作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue と pull request を管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
