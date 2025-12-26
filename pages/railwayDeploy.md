---
layout: default
title: Railwayへのデプロイ手順
---
> **注意:** この備忘録は生成AIによる未検証の内容を含みます。

# Railwayへのデプロイ手順
- Railwayは、GitHubリポジトリを連携するだけでインフラ構成を自動判別（Nixpacks）してデプロイしてくれるPaaSです。
- [Railwayの公式リファレンス](https://docs.railway.com)

## 1. Railway上でのMySQLセットアップ

1. [Railwayコンソール](https://railway.app/)にログインし、「+ New Project」をクリック。
2. 「Provision MySQL」を選択。
3. 作成されたMySQLサービスをクリックし、「Variables」タブで接続情報を確認する（後でGoから接続する際に自動連携されます）。

## 2. Goコード側の修正（重要）

Railwayで動かすために、以下の2点をコードに対応させる必要があります。

### ① ポート番号の動的取得
Railwayは環境変数 `PORT` で指定されたポートをリッスンすることを期待します。

```go
port := os.Getenv("PORT")
if port == "" {
    port = "8080" // ローカル用のデフォルト
}
// http.ListenAndServe(":" + port, nil) のように使用
```

### ② データベース接続情報の取得
- RailwayでMySQLとGoを同じプロジェクト内に置くと、Go側の環境変数に MYSQL_URL や DATABASE_URL が自動的に注入されます。

```go
// Railwayが提供する環境変数から接続情報を取得
	dbUser := os.Getenv("MYSQLUSER")
    dbPass := os.Getenv("MYSQLPASSWORD")
    dbHost := os.Getenv("MYSQLHOST")
    dbPort := os.Getenv("MYSQLPORT")
    dbName := os.Getenv("MYSQLDATABASE")
	dsn := fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?parseTime=true",
	 dbUser, dbPass, dbHost, dbPort, dbName)
```

## Railwayとの連携
- ailwayの「New Project」→「Deploy from GitHub repo」を選択
- 連携したいリポジトリを選択

## デプロイの設定
### Root Directoryを/backendにする
- デプロイ設定（Settings）の中にある 「Root Directory」 という項目を探します。

- ここを / から /backend に変更

### /backendが変更された時だけデプロイ
- Railwayで backendのサービス をクリックします。
- Settings タブを開きます。
- Deploy セクションにある Service Watch Paths という項目を探します。
- そこに以下のパスを入力して追加します。 backend/**

## データベースの作成方法
### CUI
- RailwayのMySQLインスタンスを起動した直後は、デフォルトのデータベース（通常は `railway`）のみが存在する

- MySQL -> Database -> Data -> 右上のConnect -> Publick NetWork から Raw mysql commandを確認

- ローカルのターミナルでinit.sqlがあるところまで移動，

``` bash
Raw mysql commandを実行

パスワードを入力

mysql -h tramway.proxy.rlwy.net -P {固有の数値} -u root -p  railway< init.sql

再度パスワードを入力

#パスワードは，MySQL -> Database -> Credentials に記載されているrootのパスワード
```

### 確認方法

``` bash
# パスワードを入力してDBにログイン
mysql -h tramway.proxy.rlwy.net -P {固有の数値} -u root -p

# ログインできたらSQLを打つ
mysql> SHOW DATABASES;
# データベースの名前
mysql> USE railway;
mysql> SHOW TABLES;
```

## APIのURL
- Railwayのダッシュボードを開き、Goのバックエンドサービスをクリックします。

- 上部のタブ Settings -> Networking

- https://xxx.up.railway.appという形式のURLが表示されている

- (https://がついていない場合，Next.jsには追記して伝える)

- もし表示されていない場合: 「Generate Domain」 というボタンをクリックすると、Railwayが自動でURLを発行してくれます

* [目次へ戻る](../index.md)