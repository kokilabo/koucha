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