---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

継続的な Tailspin Toys のワークフロー全体で GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、事前に用意されたフィルタリングの issue を見つけました。
- star rating を追加し、転送された browser で結果を確認し、最初の pull request（PR）を自分でマージしました。
- フィルタリングの issue から始め、Plan mode で方針を定め、Autopilot mode で実装し、Interactive mode で確認しました。
- custom instructions で agent を導き、既存の `quality-checks` skill をカスタマイズして、unit test、lint、type check を実行しました。
- Playwright Model Context Protocol（MCP）server を追加し、実際の browser でフィルタリングを確認しました。
- QA custom agent を作成して選択し、要件、coverage、skill の結果、browser の確認結果を評価しました。
- フィルタリングの変更全体を確認し、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud delegation に使える slash command を確認しました。

## 成果物

このワークショップには、更新済みの `main` からそれぞれ作成したブランチ上で、2 つの PR の節目があります。

1. **Star rating:** ゲームカードに、既存の `starRating` と、評価されていない状態を明示して表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、instructions を更新して機能に適用し、`quality-checks` の報告をカスタマイズし、QA profile を作成して関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動 test、自分での browser 確認、MCP 経由の Copilot による browser 操作など、複数の方法でコードを確認しました。`quality-checks` skill は unit test、lint、type check を実行し、新しい形式で報告しました。QA agent は PR の前に、要件と test coverage のレビューに結果をまとめました。

追加した test が実際の不足を補うべきです。QA の実行で新しい test が不要という結果になる場合もあります。マージを許可する前にコードと検証結果を確認し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストと tool が、その作業を形作ります。このワークショップでは、instructions を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。instructions は標準を設定し、skill は繰り返し可能なタスクを説明し、custom agent は専門的な役割を定義し、MCP server は外部 tool に接続します。agent の要約だけでなく、実際の変更と tool の結果を確認してください。

タスクに応じて **mode と model** を選びます。実装前に方針を考えるには **Plan**、範囲を絞った変更に参加しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速な model、複雑な作業にはより高性能な model を選びます。

インフラと同じくらい、コンテキストも重要です。何を作るのか、なぜ必要か、どのように作るのかを明確に説明すると、出力が大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` で code review agent に変更の分析を依頼します。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送信します。

## 次のステップ

どの tool も、使い続けることが上達への近道です。production code、趣味のコード、何年も作りたいと思っていた小さな app などに使ってみましょう。チームで学んだことを共有し、互いの知識から学んでください。そして、引き続きドキュメントを確認しましょう。

terminal から Tailspin Toys を拡張するには、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
