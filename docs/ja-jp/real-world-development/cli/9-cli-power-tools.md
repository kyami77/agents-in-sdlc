---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデルの選択、セッション共有、任意の cloud delegation に使える slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI tool と同様に、GitHub Copilot CLI にもさまざまな slash command があり、操作や対話に使えます。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか見ていきます。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への delegation に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の delegation など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command を使って、コンテキストを自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ開いていない場合は、リポジトリの root から Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、tool、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話がすでに短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮するタイミングを自分で決めたい場合は `/compact` を使います。無関係なタスクに切り替えるために新しい会話を始めたい場合は、代わりに `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使うモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、得た知識を共有すると、AI tool の活用方法を互いに高められます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、`/share file` によるローカルの Markdown export など、目的に合った出力先を選びます。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションの export には、prompt、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクを subagent に割り当て、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、pull request を開くことがあります。

これらのコマンドを使うと、追加の worktree やリモート作業が発生する可能性があるため、任意としています。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果を確認してください。非同期 agent 作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI の slash command を使うと、CLI の構成、セッション共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への delegation に使える任意のコマンドを確認しました。

Copilot CLI には、まだ多くの slash command があります。次は、これまでに学んだことと今後の学習方法を[振り返り][next-lesson]、このワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
