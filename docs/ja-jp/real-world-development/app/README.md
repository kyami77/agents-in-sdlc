---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションです。agent 主導の開発に必要な機能を 1 つの作業環境で利用できます。並行する agent session、切り替え可能な session mode、共有 Canvas、GitHub issue と pull request のネイティブな管理機能があります。**Agent Merge** は、rebase、review feedback、継続的インテグレーション（CI）の修正、merge まで pull request を進めます。

このワークショップでは、Tailspin Toys の一連の workflow を進めます。

1. プロジェクトを準備し、app をインストールして repository に接続し、workspace とあらかじめ用意された backlog を確認します。
2. star rating に絞った変更を行い、browser で確認して、最初の pull request（PR）を手動でマージします。
3. filtering issue から始めて、**Plan** mode で方針を定め、**Autopilot** mode で実装し、**Interactive** mode でレビューします。
4. repository instructions を更新し、filtering 作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、project check を実行します。
6. Playwright Model Context Protocol（MCP）server を追加し、browser で filtering 機能を確認します。
7. 品質保証（QA）custom agent を作成し、要件、coverage、検証結果をレビューします。
8. filtering の変更全体を確認し、2 つ目の PR に Agent Merge を使います。
9. Canvas が使えることを確認し、Database Explorer を使います。その後、repository に保存する triage Canvas を作成して test します。必要な機能が使えない場合は、blocker を記録して読み取り専用の代替手段を使うか、まとめの演習に進みます。

ワークショップを簡潔にするため、2 つの PR を作成します。1 つ目は star rating、2 つ目は filtering と instructions の更新、skill の更新、QA profile、test です。それぞれ更新済みの `main` から開始します。filtering と品質 workflow は、1 つの session、worktree、branch を使います。それぞれの tool を学びながら、前の作業を継続できます。最後の Canvas 演習は同じ session に残し、PR workflow を繰り返さずに共有 surface の作成と test に集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys project のコピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | app をインストールして project に接続し、workspace を確認します |
| [2. Star rating を追加する: 小さく始める][ex2] | 最初の変更 | 既存の rating と値がない場合の表示を追加し、PR 1 をマージします |
| [3. Agent mode: Plan と Autopilot][ex3] | Agent mode | issue から機能を計画し、Autopilot で構築して、Interactive mode で確認します |
| [4. Custom instructions で Copilot を導く][ex4] | コンテキスト | instructions を確認して更新し、filtering に適用します |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認し、報告形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | Browser での確認 | Customize から MCP を構成し、filtering の動作を確認します |
| [7. QA agent を作成して使う][ex7] | 要件と coverage | 専門 profile を作成して選択し、最終的な検証結果をまとめます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | filtering、instructions、skill、QA profile、test を確認し、2 つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | 共同作業 | Database Explorer を使い、repository に保存する triage Canvas を作成して test します |
| [10. 振り返りと次のステップ][ex10] | まとめ | workflow、成果物、参考資料を確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** plan がある GitHub account
- [ ] **macOS、Linux、Windows** のいずれかを実行する computer
- [ ] computer に[インストールされた Git][install-git]

> [!TIP]
> 有料 plan を利用していない場合でも、認証済みの学生は[GitHub Education][callout-student-plan-education]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** plan には、このワークショップで使う agent、MCP、code review、Copilot CLI の機能が含まれるため、すべての harness を完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分の computer で実行します。[前提条件の演習][ex0]では、app をインストールする前に Node.js をインストールし、project のコピーを作成する手順を説明します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が **GitHub Copilot app** policy を有効にしておく必要があります。この設定は Copilot CLI policy とは別です。

## はじめに

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
