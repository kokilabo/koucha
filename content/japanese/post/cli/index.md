---
title: 'CLIドキュメント'
date: 2025-06-25T01:22:01+09:00
draft: false
image: ''
categories:
  -
tags:
  - メモ
---

※これは個人的なメモです。

## 概要
CLIツールのドキュメント（随時追加予定）

## fd

findの代替としてシンプル、高速、使いやすい

```bash
# 基本構文
fd [PATTERN] [PATH] [OPTIONS]
```
PATTERN: 検索したいファイル名や一部の文字列（デフォルトは正規表現）

PATH: 検索対象のディレクトリ（省略時はカレントディレクトリ）

OPTIONS: 各種オプション（隠しファイル表示など）

```bash
#名前で検索
fd hello

#特定の場所から検索(homeディレクトリから)
fd config ~

#拡張子で探す
fd .txt

```
補足
パスを含めない場合現在いるディレクトリから探索を開始する

```bash
#以下のようにオプションをfdの直後に配置してもいける
fd [OPTIONS] [PATTERN] [PATH]

```

## fzf

### 基礎

```bash
# リストから選択
cat list.txt | fzf

# コマンド履歴から選ぶ
history | fzf

# Git 管理下のファイルを検索
git ls-files | fzf

# プロセスから選択
ps aux | fzf
```

### キーバインド一覧
- `Ctrl + T`：ファイル選択 → コマンドラインにパスを挿入
- `Ctrl + R`：履歴検索 → コマンドを貼り付け
- `Alt + C`：ディレクトリ選択 → `cd` で移動


### 応用：fd を使った Ctrl+T カスタム
```sh
# `~/.zshrc` に追加
export FZF_CTRL_T_COMMAND='fd --type f --hidden --follow --exclude .git'
```

- `Ctrl + T` で `.git` を除く全ファイルを `fzf` で選べる
- 隠しファイル（.dotfiles）も対象に
- `fd` を使うことで高速な検索が可能

### 使用例
```sh
vim [Ctrl + T] → ファイル選択 → Vimで開く

cp [Ctrl + T] → コピー元のファイルを選択

mv [Ctrl + T] → 移動元のファイルを選択
```

## neovim
個人的に多用しているキーバインドや特定のプラグインの用途に限定、それ以外は他のサイトを参照

> Treesitter · Telescope · LSP/Lint/Format · nvim-cmp/LuaSnip

### 1. nvim-treesitter — 構文解析とコード操作

| 機能 | コマンド / キー | 説明 |
|------|----------------|------|
|パーサ状態確認|`:TSInstallInfo`|言語ごとのインストール状況|
|パーサ追加/更新|`:TSInstall <lang>` / `:TSUpdate`|同期版は `*Sync`|
|増分選択|`gnn` → `grn / grm / grc`|ノード拡張 / 縮小 / スコープ|
|テキストオブジェクト|例 `af` (関数)|`treesitter-textobjects` が提供|
|折り畳み|`set foldmethod=expr` +<br>`foldexpr=nvim_treesitter#foldexpr()`|AST 折り畳み|

---

### 2. telescope.nvim — Fuzzy Finder

| ピッカー | コマンド | 備考 |
|----------|---------|------|
|ファイル検索|`:Telescope find_files`|Git 管理下のみなら `hidden=true`|
|ライブ Grep|`:Telescope live_grep`|`ripgrep` 必要|
|バッファ一覧|`:Telescope buffers`|開いているバッファ切替|
|ヘルプ検索|`:Telescope help_tags`|Neovim ヘルプ全文検索|

**Insert モード操作**

| キー | 動作 |
|------|------|
|`<Esc>`|終了|
|`<C-u>/<C-d>`|プレビューを上下スクロール|

---

## 3. LSP / 診断 / フォーマット / Lint

### 3-1. キーマップ（バッファローカル）

| キー | 機能 |
|------|------|
|`gd`|定義へジャンプ|
|`K`|ホバードキュメント|
|`<leader>rn`|リネーム|
|`<leader>ca`|コードアクション|
|`[d` / `]d`|前 / 次の Diagnostic|
|`<leader>e`|カーソル Diagnostic をポップアップ|
|`<leader>q`|Loclist へ Diagnostics 送信|

> **自動ポップアップ**
> カーソル停止 400 ms で `vim.diagnostic.open_float()` が表示。
> 無効化する場合は
> ```lua
> vim.diagnostic.config({ float = { focusable = false } })
> ```

---

### 3-2. フォーマッター (conform.nvim)

| Filetype | 使用ツール |
|----------|-----------|
|Python|`ruff_format` → `black`|
|Lua|`stylua`|
|C/C++|`clang-format`|
|Go|`goimports`|
|Rust|`rustfmt`|
|JS/TS|`prettierd`|
|その他|`trim_whitespace`|

*保存時に自動実行*（`format_on_save = true`）。
手動: `:lua require('conform').format()`
状態確認: `:ConformInfo`

---

### 3-3. リンター (nvim-lint)

| Filetype | Linter (PATH 必須) |
|----------|-------------------|
|Python|`ruff`|
|Lua|`luacheck`|
|C/C++|`clang-tidy` ← Mason未収録|
|Go|`golangci-lint`|
|Rust|`clippy` ← `rustup component add clippy`|
|JS/TS|`eslint_d`|

`BufWritePost` / `BufReadPost` で自動。
手動: `:lua require('lint').try_lint()`

---

### 3-4. 外部ツール管理

| 目的 | コマンド |
|------|---------|
|Mason UI|`:Mason`|
|ツール一括 Install|起動時自動 / `:MasonToolsInstall`|
|ツール更新|`:MasonToolsUpdate`|

---

### 4. nvim-cmp + LuaSnip — 補完 & スニペット

| キー | 動作 |
|------|------|
|`<C-n>/<C-p>`|次 / 前候補|
|`<C-Space>`|候補メニュー表示|
|`<CR>`|確定 (選択無しでも挿入)|
|`<Tab>/<S-Tab>`|候補選択 or スニペットジャンプ|
|`<Tab>` (行頭)|スニペット展開|

**スニペットの追加**

```lua
-- ~/.config/nvim/snippets/python/print.lua
return {
  s("pf", fmt("print({})", { i(0) })),
}


