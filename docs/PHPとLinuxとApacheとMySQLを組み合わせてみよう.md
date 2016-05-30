<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [PHPとは](#php%E3%81%A8%E3%81%AF)
  - [PHPって何？](#php%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
- [PHPで動的なWebページを作成してみよう](#php%E3%81%A7%E5%8B%95%E7%9A%84%E3%81%AAweb%E3%83%9A%E3%83%BC%E3%82%B8%E3%82%92%E4%BD%9C%E6%88%90%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [PHPのインストールと動作確認](#php%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%A8%E5%8B%95%E4%BD%9C%E7%A2%BA%E8%AA%8D)
  - [PHPとApacheの連携](#php%E3%81%A8apache%E3%81%AE%E9%80%A3%E6%90%BA)
  - [PHPの書き方](#php%E3%81%AE%E6%9B%B8%E3%81%8D%E6%96%B9)
  - [なんか動かないんスけど...](#%E3%81%AA%E3%82%93%E3%81%8B%E5%8B%95%E3%81%8B%E3%81%AA%E3%81%84%E3%82%93%E3%82%B9%E3%81%91%E3%81%A9)
  - [PHPの使い方について](#php%E3%81%AE%E4%BD%BF%E3%81%84%E6%96%B9%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)
- [LAMPをすべて組み合わせて簡単なフォームを作ってみよう](#lamp%E3%82%92%E3%81%99%E3%81%B9%E3%81%A6%E7%B5%84%E3%81%BF%E5%90%88%E3%82%8F%E3%81%9B%E3%81%A6%E7%B0%A1%E5%8D%98%E3%81%AA%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%92%E4%BD%9C%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [作成するもの: ログインフォーム](#%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B%E3%82%82%E3%81%AE-%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0)
  - [HTMLでフォームを組み立てよう](#html%E3%81%A7%E3%83%95%E3%82%A9%E3%83%BC%E3%83%A0%E3%82%92%E7%B5%84%E3%81%BF%E7%AB%8B%E3%81%A6%E3%82%88%E3%81%86)
  - [PHPでサーバーサイドの処理を実装しよう](#php%E3%81%A7%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%B5%E3%82%A4%E3%83%89%E3%81%AE%E5%87%A6%E7%90%86%E3%82%92%E5%AE%9F%E8%A3%85%E3%81%97%E3%82%88%E3%81%86)
- [課題](#%E8%AA%B2%E9%A1%8C)
- [より詳しい情報](#%E3%82%88%E3%82%8A%E8%A9%B3%E3%81%97%E3%81%84%E6%83%85%E5%A0%B1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->




# PHPとは

LAMPのPです。

## PHPって何？

正式名称は `PHP: Hypertext Preprocessor`です。

`Hypertext Preprocessor`を名乗るだけあって、主にHTMLを動的生成するWebアプリケーション用途に重きをおいたスクリプト言語です。

# PHPで動的なWebページを作成してみよう

## PHPのインストールと動作確認

`php54`パッケージをインストールして下さい。
ついでに、後ほど使うので`php54-pdo`や`php54-mysql`も入れておいてください。

```
yum install php54 php54-pdo php54-mysql
```

Apacheの`DirectoryIndex`の設定の先頭に`index.php`を追加しておいてください。
```
DirectoryIndex index.php index.html
```

PHPインストール時に、ApacheのPHP関係のモジュール、設定ファイルが自動的に読み込まれるように設定されるので、
Apacheを再起動してPHPのモジュールを読み込ませます。

```
service httpd restart
```

とりあえず、PHPファイルを作成してみましょう。

```
vim /var/www/html/index.php
---
<?php phpinfo();?>
---
```

それでは、`http://{サーバーのPublicIP}/`にアクセスしてみましょう。
下図のような画面が表示されたら成功です。お疲れ様です。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/phpinfo.png">
![phpinfo](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/phpinfo.png)
</a>

ちなみに、この[phpinfo](http://php.net/manual/ja/function.phpinfo.php)とは、
PHPの内部的な設定状況を出力する関数で、PHPが正しく動いているか確認する手段としてよく利用されます。
なにが書いてあるのかは、難しいのでわからなくて大丈夫です。

## PHPとApacheの連携

現在の状況を概念図に落とすと下図のように表すことができます。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/apache_php_arch.png">
![PHPとApacheの連携](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/apache_php_arch.png)
</a>

つまり、Apacheは、Webブラウザから`index.php`をリクエストされた際に、
`index.php`をそのままWebブラウザへ返すのではなく、`index.php`をPHP処理系で実行した結果を返しているのです。

なので、`<?php phpinfo();?>`という文字列自体でなく、`<?php phpinfo();?>`　の処理結果が表示される次第です。

## PHPの書き方

冒頭でも説明したとおり、PHPはHTMLを動的生成ことに重きをおいています。

そのため、PHPのコードはHTMLという別の言語の中に埋め込まれることが多々あるため、
特別なタグ`<?php {{PHPのコード}} ?>`で囲んで、その中にPHPの処理内容を記述します。

参考: [PHP タグ](http://php.net/manual/ja/language.basic-syntax.phptags.php)

## なんか動かないんスけど...

なにかエラーが起きているかもしれません。
エラーログ(なにかマズいことが起きた際にエラーが出力される)を確認して見ましょう。

```
tailf /var/log/httpd/error_log
```

ちなみに、アクセスログ(正常時もアクセスの履歴が残るファイル)は下記になります。

```
tailf /var/log/httpd/access_log
```

## PHPの使い方について

PHPの使い方だけでも分厚い本がかけてしまうレベルなので、まずは下記を参考にしてください。

[PHP入門 - 基本構文の解説からデータベースへのアクセス方法まで](http://www.phpbook.jp/tutorial/)



# LAMPをすべて組み合わせて簡単なフォームを作ってみよう

## 作成するもの: ログインフォーム

以下の要件のログインフォームを考えます。

* ページを開くと、ユーザ名とパスワードを入力するフォームと、ログインボタンが表示される
* ユーザー名とパスワードを入れてログインボタンを押下すると、サーバー内でログイン処理が実行される
    * データベースを検索して、合致するレコードが存在した場合、ログイン成功
    * データベースを検索して、合致するレコードが存在しない場合、ログイン失敗

ただし、前提条件として、MySQLのデータベースに以前作成したような以下のようなユーザー情報テーブルがあると想定します。

* データベース名: `form`
* テーブル名: `user_info`

| name      　　 | password      |
| :-------------: |:-------------:|
|　user1   　　  | user1         |
|　user2   　　  | user2         |
|　user3   　　  | user3         |

アクセス時

![アクセス時](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/login_form.png)

ログイン成功時

![ログイン成功時](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/login_success.png)

ログイン失敗時

![ログイン失敗時](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/login_fail.png)

## HTMLでフォームを組み立てよう

下記のファイルにフォームを表示するためのHTMLを記述します。
以前作成したファイルが残っていたら、上書きして書き換えてしまってください。

```
vim /var/www/html/index.php
------
<!doctype html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>login form</title>
  </head>
  <body>
      <form id="loginForm" name="loginForm" action="#" method="POST">
          <legend>ログインフォーム</legend>
          <label for="username">ユーザ名</label>
          <input type="text" id="username" name="username" value="" />
          <br />
          <label for="password">パスワード</label>
          <input type="password" id="password" name="password" value="" />
          <br />
          <input type="submit" id="login" name="login" value="ログイン" />
      </form>
  </body>
</html>
------
```

`http://{{サーバーのIP}}/`にアクセスした際に、ログインフォームが表示されたら成功です。
まだ、ログイン処理を実装していないので、ボタンを押しても反応がありません。

それぞれのタグの意味については、下記の参考サイトなどを調べてみてください。

参考: [HTMLタグリファレンス](http://www.htmq.com/html/index.shtml)

よく見ると、`form`タグの中に

* `input type="text"` => ユーザ名入力欄(ただの文字列入力欄)
* `input type="password"` => パスワード入力欄([ソーシャルエンジニアリング](http://e-words.jp/w/%E3%82%BD%E3%83%BC%E3%82%B7%E3%83%A3%E3%83%AB%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0.html)予防のために打った文字列が隠される文字列入力欄)
* `input type="submit"` => `form`をWebブラウザからWebサーバーへ投稿するためのボタン
* `label` => 説明用の文字列

といった要素があることが読み取れます。

このようにHTMLでは、タグ`例: <tag></tag>`を入れ子状に組み立てていくことによって、ページの内容を組み立てていきます。

## PHPでサーバーサイドの処理を実装しよう

先ほど作成した`index.php`を下記の通り書き換えましょう。

```
vim /var/www/html/index.php
------
<!-- ↓ここから追加↓ -->
<?php header("X-XSS-Protection: 0");?>
<?php
    $logged_in = false;

    // ログインボタンが押された場合      
    if (isset($_POST["login"])) {
        $username = $_POST["username"];
        $password = $_POST["password"];
        try {
            $dbh = new PDO('mysql:dbname=form;host=localhost', 'root', '');
        } catch (PDOException $e) {
            exit('データベースに接続できませんでした'.$e->getMessage());
        }
        $sql = "SELECT * FROM user_info WHERE name='$username' AND password='$password'";
        $stmt = $dbh->prepare($sql);
        $stmt->execute();
        $data = $stmt->fetch(PDO::FETCH_ASSOC);
        if (!empty($data)) {
            $logged_in = true;
            echo "ログイン成功";
        } else {
            $logged_in = false;
            echo "ログイン失敗";
        }
    }
?>
<!-- ↑ここまで追加↑ -->

<!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>login form</title>
    </head>
    <body>
        <!-- ↓この行を変更↓ -->
        <form id="loginForm" name="loginForm" action="<?php print($_SERVER['SCRIPT_NAME']); ?>" method="POST">
        <!-- ↑この行を変更↑ -->
            <legend>ログインフォーム</legend>
            <label for="username">ユーザ名</label>
            <input type="text" id="username" name="username" value="" />
            <br />
            <label for="password">パスワード</label>
            <input type="password" id="password" name="password" value="" />
            <br />
            <input type="submit" id="login" name="login" value="ログイン" />
        </form>
        <!-- ↓ここから追加↓ -->
        <?php if ($logged_in): ?>
            <?php echo "こんにちは".$_POST["username"]."さん"?>
        <?php elseif (!empty($_POST["username"])):?>
            <?php echo $_POST["username"]."さんが見つかりませんでした。入力情報を確認してください"?>
        <?php endif;?>
        <!-- ↑ここまで追加↑ -->
    </body>
</html>
------
```

噛み砕いて見ていきましょう。

### フォーム入力内容の投稿先設定

```
<form ... action="<?php print($_SERVER['SCRIPT_NAME']); ?>" ... >
```

この部分は、HTMLのfromタグのaction属性として自分自身のPHPファイル名を設定している記述です。
つまり、Webブラウザ上のログインボタンを押下した際に、
フォームに入力された情報をこのPHPファイル`index.php`で処理させるための指定です。

#### 参考

* [HTMLの基本 - 属性を指定する](http://www.htmq.com/htmlkihon/003.shtml)
* [<FORM> …… 入力・送信フォームを作成する](http://www.htmq.com/html/form.shtml)
* [$_SERVER - SCRIPT_NAME](http://php.net/manual/ja/reserved.variables.server.php)

### フォーム入力内容の投稿方法設定

```
<form ... method="POST">
```

同様に、HTMLのformタグのmethod属性として`POST`を指定する記述です。

WebサーバーとWebブラウザ間の通信に使用しているHTTPというプロトコル(通信方法に関する決まり事)には、
単純にWebサーバーからWebページを取得するだけのリクエストの他にも、
Webサーバーへ情報を送るリクエストなど、たくさんのリクエストが定められています。
それらのリクエスト種別を指定する項目がHTTPメソッドです。

今回は、ログインボタンを押下した時に、フォーム入力情報をWebサーバーへ送信したいので、
`POST`メソッドで情報をサーバーへ送るように設定します。

#### 参考

* [HTTP メソッド一覧](http://www.tohoho-web.com/ex/http.htm#methods)


### Webブラウザ上で入力された文字列の取得

前項までの設定により、ログインボタンを押下した後に、
フォーム入力内容がApacheを介してPHPへ送信されるようになりました。
ここからはフォーム入力内容の受け取り方、受け取ったあとの処理をみていきます。

PHPでは、POSTメソッドにより送信された情報は[$_POST](http://php.net/manual/ja/reserved.variables.post.php)という変数に格納されています。

見覚えがある感じの変数ですが、実は、PHPには[スーパーグローバル](http://php.net/manual/ja/language.variables.superglobals.php) というスーパーな変数があり、
 `$_SERVER` や `$_POST` のような名前の変数に、アクセス時の情報が自動的に格納されるようになっています。

なので、`$_POST["username"]`や`$_POST["password"]`のような記述により、
フォーム入力内容が取得できます。

### そのログインボタンは押されたか

実は、この`index.php`ファイルが実行されるのは、以下の2つのケースのいずれかです。

1. ユーザーがWebブラウザにURLを入力して、最初にログインフォームを表示する場合（ログインボタンは押下されていない）
1. ユーザーがログインボタンを押下してログイン処理を行った場合(ログインボタン押下された)

自明ですが、`1.` の場合、まだユーザーはログイン情報を入力していないため、
データベースでSELECT文を実行する必要はありません。

そのため、
以下の[if文](http://www.phpbook.jp/tutorial/if/index1.html)で、ログインボタンが押されたのかどうかを判定しています。

```
    // ログインボタンが押された場合      
    if (isset($_POST["login"])) {
        // ...
    }
```

すでに説明しましたが、`2.` の場合のログインボタン押下後にフォーム入力情報をWebサーバーへ送信する際は、
HTTPの`POST`メソッドで送信するようにしました。

対して、`1.` の場合のWebブラウザでいつもどおりWebサイトへアクセスする場合は、`GET`メソッドでリクエストが送信されます。

したがって、上記の様なif文でログインボタン押下有無の判定を行うことができます。

### PHP <-> MySQLサーバーの連携

PHPからMySQLサーバーとのやり取りを行うためには、
PHPの[PDO](http://php.net/manual/ja/book.pdo.php)というライブラリを使用します。

とはいっても、処理内容としては、以前ターミナルから手動実行してもらったような作業を
プログラムから行うようにしたというという程度です。

以下の行でmysqlサーバーへログインします。

```
try {
    $dbh = new PDO('mysql:dbname=form;host=localhost', 'root', '');
} catch (PDOException $e) {
    exit('データベースに接続できませんでした'.$e->getMessage());
}
```

この`try-catch`という記述は[例外処理](http://php.net/manual/ja/language.exceptions.php)といい、
名前通りに予期せぬ自体(例外)が起きた際の処理を記述するための記法です。

このコードの場合、何らかの原因(データベースサーバーが起動していなかった、データベースログインのパスワードが間違っていた)
により、データベースサーバーにログインできなかった場合に諦めてプログラムを終了するという内容になっています。

```
$sql = "SELECT count(*) as cnt FROM user_info WHERE name='{$username}' AND password='{$password}'";
$stmt = $dbh->prepare($sql);
$stmt->execute();
$data = $stmt->fetch(PDO::FETCH_ASSOC);
```

これらの行では、データベースサーバーでSELECT文を実行して、その結果をPHP側に取得してくる処理になります。
`prepare`ってなんじゃ？`PDO::FETCH_ASSOC`っておいしいの？という気分かもしれませんが、
マニアックな話題なので解説しません。興味がある方は下記を参照してください。

#### 参考

* [プリペアドステートメント](http://php.net/manual/ja/pdo.prepared-statements.php)
* [fetch_style](http://php.net/manual/ja/pdostatement.fetch.php)

### ログイン処理結果の出力

下記のとおり判定を行っています。

* SELECT文の結果が0件だったら => ログイン失敗
* SELECT文の結果が0件ではなかったら => ログイン成功

```
if (!empty($data)) {
    echo "ログイン成功";
} else {
    echo "ログイン失敗";
}
```

# 課題

前述したように、PHPには[スーパーグローバル](http://php.net/manual/ja/language.variables.superglobals.php)というスーパーな変数が存在します。

下記のデバッグコードを追加して、これらのスーパーグローバル変数の具体的な内容について、どんな情報が格納されているか確認してみましょう。

1. ユーザーがWebブラウザにURLを入力して、最初にログインフォームを表示する場合（ログインボタンは押下されていない）
1. ユーザーがログインボタンを押下してログイン処理を行った場合(ログインボタン押下された)

```
    ...
    <body>
        ...
        <h3>$_SERVER</h3>
        <pre><?php var_dump($_SERVER); ?></pre>
        <h3>$_POST</h3>
        <pre><?php var_dump($_POST); ?></pre>
    </body>
</html>
```

# より詳しい情報

[フォームデータを送信する - MDN](https://developer.mozilla.org/ja/docs/Web/Guide/HTML/Forms/Sending_and_retrieving_form_data)
