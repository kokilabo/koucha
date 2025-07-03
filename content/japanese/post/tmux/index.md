---
title: 'Tmuxのメモ'
date: 2025-06-26T01:22:47+09:00
draft: false
image: ''
categories:
  -
tags:
  - メモ
---

※これは個人的なメモです

# 📚 tmux メモ

tmuxはターミナル上で複数の作業空間（セッション・ウィンドウ・ペイン）を効率的に扱うためのツールです。

---

## 🧠 概要構造（セッション・ウィンドウ・ペイン）

```markdown
セッション1（作業A）
├─ ウィンドウ0（エディタ）
│   └─ ペイン1
├─ ウィンドウ1（サーバー）
│   ├─ ペイン1
│   └─ ペイン2

セッション2（作業B）
├─ ウィンドウ0（デバッグ）
│   └─ ペイン1
```

---

## 📌 セッション操作

```bash
# セッション一覧を表示
tmux ls

# セッションの新規作成（無名）
tmux

# セッションの新規作成（名前つき）
tmux new -s session-name

# セッションに接続（アタッチ）
tmux attach -t session-name
tmux a -t session-name  # 省略形

# セッションの終了（中で exit または Ctrl-D）
# または、別セッションから強制終了
tmux kill-session -t session-name
```

---

## 🪟 ウィンドウ操作（prefix = Ctrl-b）

```bash
# 新しいウィンドウを作成
<prefix> c

# 次のウィンドウへ移動
<prefix> n

# 前のウィンドウへ移動
<prefix> p

# ウィンドウ番号で直接移動（例：ウィンドウ0）
<prefix> 0

# ウィンドウの名前を変更
<prefix> ,
```

---

## 📐 画面分割（ペイン操作）

```bash
# 横に分割（上下に並べる）
<prefix> "

# 縦に分割（左右に並べる）
<prefix> %

# ペイン間を移動（方向キー）
<prefix> ↑ ↓ ← →
```

---

## 🔁 設定ファイルの再読み込み

```bash
# .tmux.conf を編集した後に反映
tmux source-file ~/.tmux.conf
```

---

## 🧩 プラグイン

### Extrakto

Extraktoは、画面出力からパス・URLなどを抽出し、`fzf` で操作できるプラグイン。

```bash
# Extrakto を起動（デフォルト）
<prefix> Tab
```

---

## 💡 小技（Tips）

### 🔍 出力内容のコピー

```bash
# コピー（スクロール）モードに入る
<prefix> [

# カーソルで選択範囲を移動
↑ ↓ ← → または Vimモードで h j k l

# 選択開始
スペースキー

# 選択終了してコピー
Enter
```

### 🔎 パス検索の一例

```bash
fd . | grep 'some/path/pattern'
```
