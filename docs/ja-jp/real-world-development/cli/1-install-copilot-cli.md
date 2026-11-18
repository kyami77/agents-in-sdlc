---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本を確認して、用意されたフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、terminal で動作する agent 型の coding assistant です。codebase の調査、コードの生成、command の実行、外部 tool との連携をすべて command line から行えます。task を任せたり、変更を依頼したりしながら、集中して作業できます。最初の手順として、すでに使い慣れている tool を使って Copilot CLI をインストールします。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub account で認証します。
- workshop repository を信頼して、簡単な会話を試します。
- 組み込みの GitHub MCP server で filtering issue を見つけます。

## シナリオ

チームでは、増え続ける backlog に対応するため AI agent を使い始めています。Copilot CLI は、開発者が普段使う terminal にその機能をもたらします。このレッスンでは、Copilot CLI をインストールして認証し、ワークショップの残りで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がインストール済みなので、ここでは npm を使います。

1. Codespace に戻り、terminal を開きます。
2. Node.js がインストールされ、必要な version であることを確認します。

   ```bash
   node --version
   ```

   version 24 以上が表示されます。

3. Copilot CLI を global にインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   version 番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI は GitHub account での認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. prompt が表示された場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次の prompt が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示された path が Tailspin Toys repository であることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼を確定します。

> [!NOTE]
> Codespace では、GitHub session を通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本を確認する

通常の shell prompt で入力した command は、Codespace で直接実行されます。Copilot CLI を起動すると、自然な言葉で agent に指示し、slash command で会話を操作できます。

1. `/model` を入力します。矢印キーで **Auto** を選び、<kbd>Enter</kbd> を押して、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済み version の command を確認します。<kbd>Esc</kbd> を押して help 画面を閉じます。
3. Copilot に簡単な質問をして、すべてが動作することを確かめます。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答の前に repository を確認する様子を見ます。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が利用できることを確認します。
6. Copilot に filtering issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の変更を行う際に使います。

> [!TIP]
> 通常の Copilot CLI session は、terminal で現在 checkout している branch を使います。自動的に worktree を作成することはありません。各変更の前に feature branch を作成します。

## ワークショップ用の shortcut を使う

Copilot CLI は、通常、設定済みの permission の範囲外で tool を使う前に確認を求めます。このワークショップでは、`--yolo` を使って再起動します。この shortcut は Codespace 内で承認 prompt を省略し、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は完全な自動 permission（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM のような分離された環境でのみ使用し、日常の開発で既定値として alias に設定しないでください。詳しくは [Allowing and denying tool use][allow-all-warning] を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を指定すると、後のレッスンで issue や pull request を操作するために使う GitHub MCP の読み取り / 書き込み tool が有効になります。Codespace からローカル computer へのアクセスは制限されていますが、認証済みの GitHub resource は実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. repository の root から再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確かめるため、プロジェクトについて簡単な質問をします。その後 `/exit` で終了します。

Copilot は会話を自動的に保存します。後で instructions を更新したり agent を追加したりした後、`copilot --resume` で同じ feature の会話と branch に戻ります。

## まとめと次のステップ

GitHub Copilot CLI をインストールして認証できました。このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub account で認証しました。
- workshop repository を信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP server で filtering issue を見つけました。

次は、[最初の変更][next-lesson]に取り組み、Copilot CLI でゲームカードに star rating を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
