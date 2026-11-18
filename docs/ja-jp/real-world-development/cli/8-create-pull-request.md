---
title: "レッスン 8 - 機能の pull request を作成してマージする"
description: "フィルタリング、instructions、skill の更新、QA profile、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instructions の更新、skill の更新、品質保証（QA）profile、テストを、1 つのブランチに保存しました。まとめて確認し、pull request を作成しましょう。star rating の pull request（PR）は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、instructions の更新、skill の更新、QA agent を、いくつかの PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質ワークフローに関する作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングの一連の作業を通じて、Copilot を使って機能を計画、実装、検証してきました。Tailspin Toys は、マージの許可を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げているもの（失敗した継続的インテグレーション（CI）チェックの修正、レビューコメントへの対応、必要な場合の rebase など）に取り組みます。その後、リポジトリで許可されていれば GitHub の auto-merge を有効にします。

これまでは、自分で **Pull request をマージ**を選択していました。Agent Merge にその作業を任せることもできますが、有効にする前に、変更を確認し、マージしてよい状態であると判断してください。

## Agent Merge で PR を管理する

すべてのコードができたので、まとめて確認し、PR を作成して、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を選択します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、または不足している権限を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの複数の作業を自動化しました。次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、任意の cloud delegation に関する[便利な Copilot CLI slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
