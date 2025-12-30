---
layout: default
title: Railwayのデータベースのユーザとして権限を制限したユーザーを使用する方法
---
> **注意:** この備忘録は生成AIによる未検証の内容を含みます。

# Railwayのデータベースのユーザとして権限を制限したユーザーを使用する方法

## 1. RailwayのMySQLにユーザーを作成，権限を制限

1. ローカルのターミナルから以下を実行

``` bash
# パスワードは，MySQL -> Database -> Credentials に記載されているrootのパスワード
# パスワードを入力してDBにログイン
mysql -h tramway.proxy.rlwy.net -P {固有の数値} -u root -p

# ログインできたらSQLを打つ
# ユーザーを作成
CREATE USER '作成するユーザー名(goalBingo_rw)' IDENTIFIED BY '自分で設定するパスワード';

# 作成したユーザーに権限を付与
GRANT {付与したい権限(SELECT, INSERT, UPDATE, DELETE)} ON {権限の範囲,railwayはデータベース名(railway.*)} TO '作成したユーザー名(goalBingo_rw)'@'%';
```

2. 権限を確認

``` bash
# 結果の見方
# USAGE は権限なし
# 付与した権限が2行目とかにあれば良い
SHOW GRANTS FOR '作成したユーザー名(goalBingo_rw)'@'%';
```

## 2. Railwayの環境変数を変更
1. RailwayのGoをデプロイしたサービスの環境変数を変更
2. MYSQLUSER: {作成したユーザー名(goalBingo_rw)}
3. MYSQLPASSWORD: {設定したユーザーのパスワード}

* [目次へ戻る](../index.md)