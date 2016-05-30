
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [MySQLとは](#mysql%E3%81%A8%E3%81%AF)
  - [MySQLって何？](#mysql%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [そもそもRDBMSって何？](#%E3%81%9D%E3%82%82%E3%81%9D%E3%82%82rdbms%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [データベースの構造](#%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%81%AE%E6%A7%8B%E9%80%A0)
  - [RDBMSの利点](#rdbms%E3%81%AE%E5%88%A9%E7%82%B9)
  - [SQLって何？](#sql%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
- [データベース（MySQL）に触れてみよう](#%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%EF%BC%88mysql%EF%BC%89%E3%81%AB%E8%A7%A6%E3%82%8C%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [MySQLのインストール](#mysql%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
  - [MySQLにログインしてみよう](#mysql%E3%81%AB%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [データベース・テーブルを作ってみよう](#%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E3%83%BB%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [テーブルにレコードを追加してみよう](#%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%81%AB%E3%83%AC%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E8%BF%BD%E5%8A%A0%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [追加したレコードを検索してみよう](#%E8%BF%BD%E5%8A%A0%E3%81%97%E3%81%9F%E3%83%AC%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E6%A4%9C%E7%B4%A2%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
- [課題](#%E8%AA%B2%E9%A1%8C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# MySQLとは

LAMPのMです。

厳密に説明しようとすると、大学院の「データベース特論」みたいな講義レベルになってしまうので、
簡単のために、あまり正確でない説明が含まれています。

詳しい定義などは、[Wikipedia](https://ja.wikipedia.org/wiki/Template:Database)などを参照してください。


## MySQLって何？

[関係データベース管理システム(RDBMS)](https://ja.wikipedia.org/wiki/%E9%96%A2%E4%BF%82%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)という種類に属する[データベース管理システム(DBMS)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)のミドルウェアです。

MySQLという名前からもわかるように、
[SQL](https://ja.wikipedia.org/wiki/SQL)というデータベース向け言語を用いて、データの管理を行うことができます。

## そもそもRDBMSって何？

社会一般で用いられている[データベース](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9)として、
下記のようなモノがあります。

- 住所録
- 電子カルテ
- 企業情報データベース
- ...

これらのデータベースをサーバー上に構築するためのミドルウェアがDBMSです。

さらに、住所録の例を思い浮かべると、`氏名、住所、電話番号...`というような一定の表形式で記述されていることが多いと思います。  
このような、一定の表形式でデータを管理するDBMSを、特にRDBMSと呼んだりします。

今日のWebサービスの大半は、何らかのDBMSを使用してデータ管理を行っているはずです。

## データベースの構造

一般的なRDBMSにおけるデータベースは、基本的に下記のようなデータ構造となっています。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/DB_image.png">
![データベースの構造イメージ](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/DB_image.png)
</a>

* データベース(図中の`ECサイトのデータベース`など)という大きな箱がある
* データベースの中に、テーブル(図中の`ユーザ`など)が存在する
* テーブルは表形式で表される
    * 行: レコード
    * 列: カラム


## RDBMSの利点

「そんなもの使わなくても、データを保存するだけならば、適当にファイルへ書き込んでおけば良いじゃないか?」  
という気がしてきます。

RDBMSを使う利点とは何なのでしょうか。

* SQLというデータベース言語を使用して、簡単にデータ操作処理(データ検索・保存...)を記述できる
* データ記録処理（[永続化](http://d.hatena.ne.jp/keyword/%B1%CA%C2%B3%B2%BD)処理とも）の詳細を隠蔽、抽象化できる
    * 「記録場所を サーバー内のファイル から 遠隔地のデータベースサーバー に変更する」のような変更を、アプリケーションの改修をせずに、データベースの設定を変更するだけでサクッとできる
* データ形式の制約を設定できる
    * ”氏名欄が入力されていない住所録データ”のような不正なデータを検査・検知できる
* [その他...](https://ja.wikipedia.org/wiki/%E9%96%A2%E4%BF%82%E3%83%87%E3%83%BC%E3%82%BF%E3%83%99%E3%83%BC%E3%82%B9%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0#RDBMS.E3.81.AE.E6.A9.9F.E8.83.BD)

要するに、よくあるデータ管理に関わる処理を[車輪の再発明](https://ja.wikipedia.org/wiki/%E8%BB%8A%E8%BC%AA%E3%81%AE%E5%86%8D%E7%99%BA%E6%98%8E)せずに、
アプリケーションに組み込むことができることが一番の利点だと言えると思います。

## SQLって何？

MySQLをはじめとしたRDBMSが理解できる言語です。
MySQLに対して命令(あのデータ検索しろ、このデータを追加しろ...)を伝える際には、SQLという言語により命令内容を記述します。

命令の種類によって、以下に述べる3種類の言語要素があります。

それぞれの要素について、詳しい説明は[Wikipedia](https://ja.wikipedia.org/wiki/SQL)を参照してください。

### [データ定義言語(DDL)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E5%AE%9A%E7%BE%A9%E8%A8%80%E8%AA%9E)

データベースに格納するデータの構造定義に関する命令です。

* CREATE文: データベースやテーブルの構造の定義を行う
* DROP文: 既存のデータベースやテーブルを削除する
* ALTER文: 既存のデータベースやテーブルの構造を変更する

### [データ操作言語(DML)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E6%93%8D%E4%BD%9C%E8%A8%80%E8%AA%9E)

あらかじめ定義したデータベースのテーブルに対して、下記のような操作を行う命令です。

* SELECT文: レコードを検索する
* INSERT文: レコードを新規登録する
* UPDATE文: レコードを更新する
* DELETE文: レコードを削除する

### [データ制御言語(DCL)](https://ja.wikipedia.org/wiki/%E3%83%87%E3%83%BC%E3%82%BF%E5%88%B6%E5%BE%A1%E8%A8%80%E8%AA%9E)

あらかじめ定義したデータベースのテーブルに対して操作を行うための権限の定義に関する命令です。

* GRANT文: 指定したユーザーに特定の作業を行う権限を与える
* REVOKE文: 指定したユーザーから既に与えた権限を剥奪する

# データベース（MySQL）に触れてみよう

実際にMySQLにログインしてSQL文を実行してみよう。

参考: [MySQLの使い方](http://www.dbonline.jp/mysql/)

## MySQLのインストール

下記の2つのパッケージをインストールします。

* `mysql-server`: MySQLサーバー本体
* `mysql`: MySQLサーバーに接続して作業するためのクライアント

```
yum install mysql mysql-server
```

例によって、自動起動の設定と`mysql-server`の起動を行います。
`mysql-server`は、サーバー上では`mysqld`と表記されている場合が多いです。

```
chkconfig mysqld on
service mysqld start
```

## MySQLにログインしてみよう

ターミナルで下記のコマンドを入力して、MySQLにログインします。

```
mysql
```

なにやらずらずら表示されますが、`mysql>`のような表示(mysqlのプロンプト)がでていたらログイン成功です。

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.5.46 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

これ以降のコマンドは、mysqlのプロンプトに対して実行します。

## データベース・テーブルを作ってみよう

下記のような構造のデータベース・テーブルを作成してみましょう。

* DATABASE : `form`
* TABLE    : `user_info`

| name      　　 | password      |
| ------------- |:-------------:|
|　user1   　　  | user1         |
|　user2   　　  | user2         |
|　user3   　　  | user3         |
|　user4   　　  | user4         |

### データベース作成

まず、`form`というデータベースを作ります。

`CREATE DATABASE form;`という命令実行の後に、`form`というデータベースが生成されていることがわかります。

```
# 既存のデータベース確認
SHOW DATABASES;
> +--------------------+
> | Database           |
> +--------------------+
> | information_schema |
> | mysql              |
> | performance_schema |
> | test               |
> +--------------------+
> 4 rows in set (0.01 sec)

# データベース作成
CREATE DATABASE form;
> Query OK, 1 row affected (0.00 sec)

# もう一度、データベース確認
SHOW DATABASES;
> +--------------------+
> | Database           |
> +--------------------+
> | information_schema |
> | form               |
> | mysql              |
> | performance_schema |
> | test               |
> +--------------------+
> 5 rows in set (0.00 sec)
```

### テーブル作成

次に、`form`データベース内に`user_info`テーブルを追加してみましょう。

```
# データベースに接続する
USE form
> Database changed

# 既存のテーブル確認
SHOW TABLES;
> Empty set (0.00 sec)

# テーブル生成
# VARCHAR(255) => 255桁の文字列
CREATE TABLE user_info (name VARCHAR(255), password VARCHAR(255));
> Query OK, 0 rows affected (0.01 sec)

# もう一度確認
SHOW TABLES;
> +----------------+
> | Tables_in_form |
> +----------------+
> | user_info      |
> +----------------+
> 1 row in set (0.00 sec)

# テーブルの構造を確認
DESC user_info;
> +----------+--------------+------+-----+---------+-------+
> | Field    | Type         | Null | Key | Default | Extra |
> +----------+--------------+------+-----+---------+-------+
> | name     | varchar(255) | YES  |     | NULL    |       |
> | password | varchar(255) | YES  |     | NULL    |       |
> +----------+--------------+------+-----+---------+-------+
> 2 rows in set (0.00 sec)

```

## テーブルにレコードを追加してみよう

`user_info`テーブルにレコードを挿入してみよう。

```
# レコード挿入
INSERT INTO user_info (name, password) VALUES ("user1", "user1");
> Query OK, 1 row affected (0.00 sec)

# 同様に他のレコードも挿入してみよう
# INSERT INTO user_info (name, password) VALUES ("user2", "user2");
# INSERT INTO user_info (name, password) VALUES ("user3", "user3");
# INSERT INTO user_info (name, password) VALUES ("user4", "user4");

# 挿入できたか確認
SELECT * FROM user_info;
> +-------+----------+
> | name  | password |
> +-------+----------+
> | user1 | user1    |
> | user2 | user2    |
> | user3 | user3    |
> | user4 | user4    |
> +-------+----------+
> 3 rows in set (0.00 sec)
```

## 追加したレコードを検索してみよう

SELECT文をガシガシ弄ってみよう。

```
# 全件表示
SELECT * FROM user_info;
> +-------+----------+
> | name  | password |
> +-------+----------+
> | user1 | user1    |
> | user2 | user2    |
> | user3 | user3    |
> +-------+----------+
> 3 rows in set (0.00 sec)

# nameカラムがuser1のレコードを絞り込み
SELECT * FROM user_info WHERE name = "user1";
> +-------+----------+
> | name  | password |
> +-------+----------+
> | user1 | user1    |
> +-------+----------+
> 1 row in set (0.00 sec)

# user1さんのパスワードを抜き出し
SELECT password FROM user_info WHERE name = "user1";
> +----------+
> | password |
> +----------+
> | user1    |
> +----------+
> 1 row in set (0.00 sec)
```

# 課題

下記のSQL文を調べて考えてみてください。

1. `user_info`テーブルに追加したuser4のパスワードを変更してみよう
    * user4 -> password4
2. user4のレコードを削除してみよう
