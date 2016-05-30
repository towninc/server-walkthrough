
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Webサーバーとは](#web%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%A8%E3%81%AF)
  - [Webサーバーって何？](#web%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [Webアプリケーションって何？](#web%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [LAMPって何？](#lamp%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
- [ApacheでWebサーバーを構築してみよう](#apache%E3%81%A7web%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%82%92%E6%A7%8B%E7%AF%89%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [Apacheをインストールしよう](#apache%E3%82%92%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%81%97%E3%82%88%E3%81%86)
  - [httpdの起動と動作確認](#httpd%E3%81%AE%E8%B5%B7%E5%8B%95%E3%81%A8%E5%8B%95%E4%BD%9C%E7%A2%BA%E8%AA%8D)
  - [DirectoryIndexって何？](#directoryindex%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [DocumentRootって何？](#documentroot%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
- [課題](#%E8%AA%B2%E9%A1%8C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Webサーバーとは

## Webサーバーって何？

Webサーバーとは、以前解説したサーバーのうち、
私たちが普段使用している情報端末(コンピュータ、スマートフォンなど)上のWebブラウザ（Webクライアント）に対して、
Webページのデータをサービスする役割を持つコンピュータ（または、その機能を持ったソフトウェア）です。

今日、広く使われているWebサイトは、（大雑把な説明ですが）下記のような仕組みで実現されています。

1. (ユーザー) Webブラウザ上でWebページを開く
2. (Webブラウザ) ユーザに依頼されたWebページをサービスしているWebサーバを探索
3. (Webブラウザ) 該当するWebサーバに対して、Webページのデータを要求
4. (Webサーバー) Webブラウザから要求されたWebページのデータをWebサーバー内で探索
5. (Webサーバー) Webブラウザへ見つかったデータを送り返す
6. (Webブラウザ) Webサーバーから送り返されてきたデータを組み立ててユーザーに提示する

ここで、`3.`と`5.`における、WebサーバーとWebブラウザ間のやり取りは、[HTTP (HyperText Transfer Protocol)](https://ja.wikipedia.org/wiki/Hypertext_Transfer_Protocol)という全世界共通なお約束に従って行われます。

また、`5.`で送り返されるデータも同様に、[HTML (HyperText Markup Language)](https://ja.wikipedia.org/wiki/HyperText_Markup_Language)という全世界共通な言語で記述されています。

そのため、どんなメーカーのスマートフォンでも同じようにWebページを閲覧することができるのです。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/web_app_server_arch.png">
![Webサーバーのイメージ図](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/web_app_server_arch.png)
</a>

## Webアプリケーションって何？

ここで、ひとつの疑問にぶつかります。
Webサーバーとは、”Webブラウザから要求されたデータを探しだして送り返す”役割を持っていると説明しました。

しかし、西暦201x年を生きる私達が普段利用しているインターネットは、そんなに単純なものでしょうか。

ちょっと考えるだけで、私達が享受している21世紀のインターネットは、
もっと便利で複雑な機能を持っているということがわかります。

* ECサイトで商品をカートに投入して購入をすると、商品が手元に配送されます
* GmailのようなWebメールサービスを開くと、PCやスマホのメールアプリと同様にメールの送受信を行うことができます
* わざわざWebサーバーにログインして黒い画面を弄らなくても、いまの気持ちをTwitterに投稿できます
* ...

これら21世紀の素敵なインターネットは、（例によって大雑把な説明ですが）下図のようなアーキテクチャで実装された
Webアプリケーションによって実現しています。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/web_app_server_arch.png">
![Webアプリケーションのイメージ図](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/web_app_server_arch.png)
</a>

前項のWebサーバーに加えて、下記の要素が増えました。

- WebAPサーバー(Webアプリケーションサーバー)
- DBサーバー(データベースサーバー)

これら２つのサーバーは連携して動作し、下記のような処理を行います。

- Webサーバーを介してWebブラウザからのリクエストを受けとり、リクエストに応じた処理を実行する
  - ECサイトのお客さんの注文を受け付けて、DBに受注情報を格納
  - ユーザがWebメール画面でメールを送信ボタンを押したら、サーバー側でメール送信処理を実行
  - ユーザから投稿されたツイートをDBに格納
- 処理結果をWebサーバーを返して、Webブラウザに送ってもらう
  - ユーザに購入完了画面を表示
  - メール送信完了通知を表示
  - ツイート完了したことを画面上に表示、タイムラインに投稿したツイートをのせる

このようなアーキテクチャを[3層クライアントサーバシステム](http://e-words.jp/w/3%E9%9A%8E%E5%B1%A4%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0.html)などと呼んだりします。

Webサイト・Webサービスによっては、仕組みが多少異なっていたり、
もっと複雑怪奇な構成をとっていたりしていて一概示すことはできないのですが、
基本的にこのような３層構造をとっているケースは多いです。

## LAMPって何？

Webアプリケーションサーバーを構築する際によく使用されるミドルウェア（「縁の下の力持ち」的なソフトウェア）として、
[LAMP (Linux+Apache+MySQL+PHP/Perl/Python)](http://e-words.jp/w/LAMP.html)というものがあります。

LAMPとは、下記のミドルウェアの頭文字をもじった単語です。

* Webサーバーの基本ソフトウェア: `Linux`
* Webサーバーアプリケーション: `Apache HTTP Server`
* DBサーバー: `MySQL`
* Webアプリケーションの処理系: `PHP` / `Perl` / `Python`

実は、このカリキュラムで今まで学んできた内容は"LAMP"の"L"(の基礎の基礎)だったのです。

<a href="https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/lamp_stack.png">
![LAMPスタック](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/lamp_stack.png)
</a>

LAMPスタックの上に実装されたWebアプリケーションとして、主に下記のようなものがあります。

* ブログサイトを自由に作りたい => WordPress
* ECサイトで商売したい => EC-CUBE
* （所謂）ホームページを作りたい => Drupal

# ApacheでWebサーバーを構築してみよう

「`Apache HTTP Server`」が正式名称ですが、サーバーを弄るときには「`httpd`」と記述されていることが多いです。
また、口語的に「`アパッチ`」、「`Apache`」とは、大抵「`Apache HTTP Server`」を指します。

LAMPのAです。

## Apacheをインストールしよう

### パッケージ管理ツール

Linuxの世界では、Apacheに限らない各種ミドルウェアは、パッケージという簡単にインストールできる形態で頒布されています。

このカリキュラムで使用しているAmazon Linuxでは、パッケージのインストールに[yum](https://access.redhat.com/documentation/ja-JP/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/ch-yum.html)というパッケージ管理ツールを使用します。

Yumを使用してパッケージをインストールするには、
```
yum install (インストールしたいパッケージ)
```
というコマンドを実行します。

(注) `yum install`コマンドでは、サーバーのいろいろな場所を書き換えるので、実行にあたっては管理者権限が必要になります。

でも、その前にインストールするパッケージ名を調べなくてはなりません。
なので、下記の`yum search`コマンドにより、
インストールしたいパッケージの名前をキーワード検索して調べます。
```
yum search (キーワード)
```
というコマンドを実行すると、キーワードに合致するパッケージの情報がリストアップされます。


### YumでApacheをインストールしよう

`httpd24`パッケージをインストールして下さい。

```
# パッケージ検索
yum search apache http server
# いろいろヒットしますが、
# httpd24.x86_64 : Apache HTTP Server
# をインストールしましょう
yum install httpd24
```

## httpdの起動と動作確認

まずApacheを起動します

```
service httpd start
```

ブラウザを開いて `http://{サーバーのPublicIP}/` にアクセスしてみましょう。

```
Amazon Linux AMI Test Page
```

というページが出ていれば大丈夫です。おめでとう。

このままだと、Webサーバーが再起動した際に、
またstartしなくてはならないので、サーバー起動時にApacheも自動起動するように設定しておきましょう。

```
chkconfig httpd on
```

テストページを見せられても誰も得しないので、
自分で作ったページを表示できるようにしてみましょう。

```
vim /var/www/html/index.html
---
Hello, World.
---
```

`index.html`を保存した後に、もう一度先ほどのURLにアクセスしてみてください。
自分で作ったページの内容が表示されたら成功です。

## DirectoryIndexって何？

先ほど、`/var/www/html/index.html`というファイルを作成したところ、
`http://{サーバーのPublicIP}/`でアクセスした際の表示内容が変わりました。

さらに、`http://{サーバーのPublicIP}/index.html`へアクセスした際の表示内容も同様に変わっているはずです。

ここで質問です。

`http://{サーバーのPublicIP}/index.html`
というURLは、直感的には「PublicIPのアドレスのサーバー上にあるindex.htmlというファイル」
を表していると思われます。

なぜ`http://{サーバーのPublicIP}/`というURL（ファイル名が省略されているURL）にアクセスした際に`index.html`の内容が表示されたのでしょうか。

正解は`DirectoryIndex`という設定があるからです。

[DirectoryIndex](https://httpd.apache.org/docs/current/mod/mod_dir.html#directoryindex)とは、「リクエストされたURLにおいてファイル名が省略されていた場合に、どのファイルを返すのか」
を指定する設定です。

特にそんなものを設定した記憶は無いかと存じますが、
Apacheが気を利かせて`DirectoryIndex` のデフォルト設定として、
`/var/www/html`に`index.html`が存在した際はそいつを返すように設定してくれていたのでした。

```
vim -R /etc/httpd/conf/httpd.conf
---
...
159 #
160 # DirectoryIndex: sets the file that Apache will serve if a directory
161 # is requested.
162 #
163 <IfModule dir_module>
164     DirectoryIndex index.html
165 </IfModule>
...
---
```

## DocumentRootって何？

「`index.html`については分かったが、
そもそもなんで`/var/www/html/`みたいなよくわからないディレクトリに保存しなくてはならないんだ？」

という質問が聞こえてきそうなので説明します。

[DocumentRoot](http://httpd.apache.org/docs/2.4/ja/mod/core.html#documentroot)という設定がそう設定されているからです。

```
vim -R /etc/httpd/conf/httpd.conf
---
...
114 #
115 # DocumentRoot: The directory out of which you will serve your
116 # documents. By default, all requests are taken from this directory, but
117 # symbolic links and aliases may be used to point to other locations.
118 #
119 DocumentRoot "/var/www/html"
...
---
```

`DocumentRoot`とは、どのディレクトリ以下をインターネット上に公開するか？という設定です。
特に`/var/www/html`である必要性は無いのですが、慣習的に`/var/www/html`であることが多いです。


# 課題

- `DirectoryIndex`の設定に`index.htm`を追加してみよう
    - 変更した設定を記入してください
- `DocumentRoot`の設定を`/var/www`に変更してみよう
    - 変更した設定を記入してください
    - `/var/www/index.htm`のような適当なファイルを作成してみよう

最終的に、`http://{サーバーのPublicIP}/`というURLにアクセスした時に、
`/var/www/index.htm`のファイルの内容が表示されたら成功です。

確認が終わったら、最後に、`DocumentRoot`の設定を`/var/www/html`に戻しておいてください。
