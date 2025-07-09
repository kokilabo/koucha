---
title: '便利な検索演算子'
date: 2025-07-09T17:46:00+09:00
draft: false
image: ''
categories:
  - 
tags:
  - メモ
---

## 概要
googleで使える検索演算子をまとめる。

### 🔍 基本的な検索演算子

| 演算子 | 内容 | 例 |
|--------|------|----|
| `""` | 完全一致検索 | `"検索演算子 一覧"` → その語順通りの結果だけ |
| `-` | 除外検索 | `猫 -犬` → 犬を含まない猫の情報 |
| `site:` | 特定サイト内検索 | `site:nhk.or.jp 地震` |
| `intitle:` | タイトルに含む | `intitle:Python` |
| `inurl:` | URLに含む | `inurl:login` |
| `filetype:` | ファイル形式指定 | `filetype:pdf 就職活動` |
| `OR`（大文字） | いずれかを含む | `Python OR JavaScript` |
| `*` | ワイルドカード | `"AI * 影響"` → 間に何か入るパターンを検索 |
| `..` | 数値範囲 | `iPhone 10..14` → iPhone10〜14の範囲検索 |

---

### 💡 応用・裏技っぽい使い方

| 技 | 内容 | 例 |
|----|------|----|
| `related:` | 関連サイト表示 | `related:youtube.com` |
| `cache:` | キャッシュページ表示 | `cache:example.com` |
| `"検索語1" AROUND(N) "検索語2"` | 近接検索（N語以内） | `"AI" AROUND(3) "倫理"` |
| `define:` | 単語の定義を検索 | `define:machine learning` |
| `before:` / `after:` | 日付で絞る（Google News等） | `ウクライナ after:2023-01-01` |

---
