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

### telescope.nvim — Fuzzy Finder

| よく使うピッカー | コマンド例 | 説明 |
|-----------------|-----------|------|
|ファイル検索|`:Telescope find_files`|Git 管理下のみの場合は `hidden=true` に|
|ライブ Grep|`:Telescope live_grep`|ripgrep 必須 (`brew install ripgrep`)|
|バッファ一覧|`:Telescope buffers`|開いているバッファを切替|
|ヘルプタグ|`:Telescope help_tags`|Neovim ヘルプを全文検索|

### キー操作 (Insert モード)

| キー | 動作 |
|------|------|
|`<Esc>`|プレビューを閉じて即終了|
|`<C-u>` / `<C-d>`|プレビューウィンドウのスクロール|

> プロジェクトごとに頻繁に使うピッカーは `<leader>f` 系にマッピングすると便利。

### LSP / 診断 / フォーマット / Lint

- 共通キーマップ（バッファローカル）

| キー | 動作 |
|------|------|
|`gd`|定義へジャンプ|
|`K`|ホバードキュメント|
|`<leader>rn`|シンボル名変更|
|`<leader>ca`|コードアクション|
|`[d` / `]d`|前 / 次の Diagnostic へ|
|`<leader>e`|カーソル位置の Diagnostic をポップアップ|
|`<leader>q`|バッファ Diagnostic を loclist に送る|

#### Diagnostic ポップアップ自動表示
カーソル停止後 **400 ms** で `vim.diagnostic.open_float()` が走ります。うるさい場合は

```lua
vim.diagnostic.config({ float = { focusable = false } })

