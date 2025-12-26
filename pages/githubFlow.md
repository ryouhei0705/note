---
layout: default
title: GitHub Flowでの手順
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
- 自分のPCのターミナル（またはVS Codeのターミナル）で、最新のメインブランチから作業用のブランチを作成します。

```bash
# メインブランチに移動して最新状態にする
git checkout main
git pull origin main

# 新しいブランチを作成（名前は自由。issue番号を入れると分かりやすい）
git checkout -b feature/1-make-user-page
```

## コミット・プッシュ
```bash
# 変更したファイルを追加
git add .

# メッセージを付けて保存（"Closes #1"と書くと、後で自動的にIssueが閉じます）
git commit -m "Goのデプロイメモを追加 Closes #1"

# GitHubへ送信
git push origin feature/issue-1
```

## PR
- GitHubの画面に戻ると「Compare & pull request」というボタンが出ているのでクリックします。

- 内容を確認し、「Create pull request」をクリック。

- メッセージにcloses #1を書いておくと，マージ時に自動でissueがクローズされる

- これにより、メインの記録に合流させる前の「最終確認待ち」の状態になります。

## マージする
- 自分一人での作業なら、そのまま「Merge pull request」→「Confirm merge」をクリックします。これで、書いたメモが正式に公開用ブランチに反映されます。

## issueをクローズ
- コミットメッセージに closes #1 と書いていれば、マージと同時にIssueは自動的にクローズされます。

- 書いていなかった場合は、Issueの画面を開いて 「Close issue」 ボタンを手動で押します。

## ブランチを削除
- ローカルブランチの削除

- 通常の削除

``` bash
# 別のブランチ（mainなど）に移動してから実行
git checkout main

# ブランチを削除 (-d は delete の略)
git branch -d ブランチ名
```

- 強制削除(マージ時ていない場合)

```bash
git branch -D ブランチ名
```

- リモートブランチの削除

```bash
git push origin --delete ブランチ名
```

- 削除後の整理

```bash
# リモートで消されたブランチの情報をローカルから掃除する
# -p は-prune(枝落とし)の略
git fetch -p
```
- （作業用ブランチは使い捨てにするのが基本です）

* [一覧へ戻る](../index.md)