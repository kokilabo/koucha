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

## 🚀 Neovim
<kbd>Leader</kbd> = <kbd>Space</kbd>（半角スペース） — 主要キーバインド一覧

---

### 🔍 Telescope — 検索
| キー | アクション |
|------|-----------|
| <kbd>Leader f f</kbd> | ファイル検索 |
| <kbd>Leader f g</kbd> | 全文 Grep |
| <kbd>Leader f b</kbd> | バッファ一覧 |
| <kbd>Leader f h</kbd> | ヘルプ検索 |

<details><summary>📜 Telescope 中の操作</summary>

| キー | 動作 |
|------|------|
| <kbd>Esc</kbd> | 終了 |
| <kbd>Ctrl u / Ctrl d</kbd> | プレビュー上下スクロール |

</details>

---

### 🧠 LSP — コードナビ & 診断
| キー | アクション |
|------|-----------|
| <kbd>g d</kbd> | 定義へジャンプ |
| <kbd>K</kbd> | ホバー情報 |
| <kbd>Leader r n</kbd> | リネーム |
| <kbd>Leader c a</kbd> | コードアクション |
| <kbd>] d</kbd> / <kbd>[ d</kbd> | 次 / 前の Diagnostic |
| <kbd>Leader e</kbd> | Diagnostic ポップアップ |
| <kbd>Leader q</kbd> | Diagnostic → loclist |

---

### 🌳 Treesitter
| キー | アクション |
|------|-----------|
| <kbd>g n n</kbd> | 増分選択開始 |
| <kbd>g r n / g r m</kbd> | 選択拡張 / 縮小 |
| <kbd>g r c</kbd> | スコープ単位拡張 |

---

### 🔤 nvim-cmp / LuaSnip
| キー | アクション |
|------|-----------|
| <kbd>Ctrl Space</kbd> | 補完ポップアップ |
| <kbd>Ctrl n / Ctrl p</kbd> | 次 / 前候補 |
| <kbd>Enter</kbd> | 候補確定 |
| <kbd>Tab / Shift Tab</kbd> | 候補ナビ or スニペットジャンプ |

