---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI の mode、customization、MCP tool、pull request automation を学びながら、Tailspin Toys に 2 つの変更を実装、検証して公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、terminal で GitHub Copilot を agent 型の coding assistant として利用できます。codebase の調査、コードの生成、command の実行、外部 tool への接続ができます。すべて command line から行えるため、graphical editor に切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連の workflow を進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
2. star rating に絞った変更を行い、browser で確認して、最初の pull request（PR）を手動でマージします。
3. filtering issue から始めて、Plan mode で方針を定め、Autopilot mode で実装し、Interactive mode でレビューします。
4. repository instructions を更新し、フィルタリング作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、project check を実行します。
6. Playwright Model Context Protocol（MCP）server を追加し、browser でフィルタリングを確認します。
7. 品質保証（QA）custom agent を作成して、要件、coverage、検証結果をレビューします。
8. フィルタリングの変更全体を確認し、Agent Merge を使ってフィルタリングの PR をマージします。
9. コンテキスト、model、共有、任意の cloud delegation に使える slash command を確認します。

ワークショップを簡潔にするため、2 つの PR を作成します。1 つ目は star rating、2 つ目は filtering と instructions の更新、skill の更新、QA profile、test です。filtering と品質 workflow は、1 つの会話と branch を使います。それぞれの tool を学びながら、前の作業を継続できます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | repository と Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. Star rating を追加する: 小さく始める][ex2] | 最初の変更 | 既存の rating と値がない場合の表示を追加し、最初の PR をマージする |
| [3. Agent mode: Plan と Autopilot][ex3] | Agent mode | issue から機能を計画し、Autopilot で構築して、Interactive mode で確認する |
| [4. Custom instructions で Copilot を導く][ex4] | コンテキスト | instructions を確認して更新し、フィルタリングに適用する |
| [5. quality-checks skill をカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存の skill を確認し、報告形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | Browser の確認 | CLI で MCP を構成し、フィルタリングの動作を確認する |
| [7. QA agent を作成して使う][ex7] | 要件と coverage | 専門 profile を作成して選択し、最終的な検証結果をまとめる |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体を確認し、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI 機能 | コンテキスト、model、共有、任意の cloud agent への delegation を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | workflow、再利用可能な customization、参考資料を確認する |
| [オプション: Foundry を組み込む][foundry] | Host 型 agent | model を準備し、catalog の情報に基づく agent を deploy して Web サイトに接続する |

## 前提条件

ワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** plan がある GitHub account
- [ ] repository と Codespace を作成する権限
- [ ] terminal または command line 操作の基本的な知識

> [!TIP]
> 有料 plan を利用していない場合でも、認証済みの学生は[GitHub Education][student-plan]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** plan には、このワークショップで使う agent、MCP、code review、Copilot CLI の機能が含まれます。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめに

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
