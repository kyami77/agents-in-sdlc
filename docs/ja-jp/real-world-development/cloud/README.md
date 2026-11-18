---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot が cloud 上で非同期に作業できます。GitHub でタスクを割り当てると、cloud agent はバックグラウンドでリポジトリを調べ、変更を行い、pull request を開きます。その間、別の作業を進められます。

各演習では、まず cloud agent が従う custom instructions を追加します。次に GitHub issue を割り当てて、実装を依頼します。custom agent を確認して活用し、agents dashboard からセッションを監視して方向を示し、最後に pull request をレビューして作業を反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Custom instructions][ex1] | コンテキスト | cloud agent が従う custom instructions を追加する |
| [2. Cloud agent][ex2] | 非同期 agent | issue を Copilot cloud agent に割り当てる |
| [3. Custom agents][ex3] | 専門化した agent | custom agent を確認して使う |
| [4. Agent の管理][ex4] | 監視 | agent のセッションを監視して方向を示す |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して、次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** plan がある GitHub account
- [ ] account または organization で有効になっている Copilot cloud agent

> [!TIP]
> 有料 plan を利用していない場合でも、認証済みの学生は[GitHub Education][callout-student-plan-education]を通じて GitHub Copilot を無料で利用できます。**Copilot Student** plan には、このワークショップで使う agent、MCP、code review、Copilot CLI の機能が含まれます。どの harness でも利用できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free は対象外です。Copilot Business または Enterprise では、organization の管理者が有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP server を使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/)を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
