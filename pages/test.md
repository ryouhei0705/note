---
layout: default
title:  Flowでの手順
---

# GitHub Flowでの手順

> **注意:** この備忘録は生成AIによる未検証の内容を含みます。

## issueを立てる

- GitHubのリポジトリ画面で「Issues」タブを開き、「New Issue」をクリックします。
- Title: 「Goのデプロイメモ追加」など
- Description: 何をメモするか簡単に記載
- 右側の「Labels」などで分類してもOKです。
- 作成すると #1 のようなIssue番号が割り振られます。

## ブランチを切る

- 自分のPCのターミナルで、最新のメインブランチから作業用のブランチを作成します。

```bash
# メインブランチに移動して最新状態にする
git checkout main
git pull origin main

# 新しいブランチを作成
git checkout -b feature/1-make-user-page
```