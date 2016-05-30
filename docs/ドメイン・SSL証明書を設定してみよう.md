<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [ドメインを取得してみよう](#%E3%83%89%E3%83%A1%E3%82%A4%E3%83%B3%E3%82%92%E5%8F%96%E5%BE%97%E3%81%97%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
- [ComodoFreeSSLを入れてみよう](#comodofreessl%E3%82%92%E5%85%A5%E3%82%8C%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# ドメインを取得してみよう

以前作成したWebページを開くときに、IPアドレスを使っているかと思いますが、IPアドレスではSSL証明書を発行することが出来ません。

一方、IPアドレスに紐付いているドメインならば、発行が出来ます。

ここではSSL証明書を発行するために必要なドメインを取得してみます。

今回は以下のサイトを利用して、ドメインを取得してみてください。

以下のサイトが参考になるかと思います。

[Dot TKの無料ドメインを取得する](https://blog.apar.jp/cloud/2142/)

「Route 53 Hosted Zones の作成」の手前まで行って下さい。

浸透するまで多少時間がかかるので、登録してすぐにアクセスして表示出来なくても大丈夫です。

作成したドメイン名でアクセスした時に今までIPアドレスで開けていたサイトが開ければ大丈夫です。


# ComodoFreeSSLを入れてみよう

Webサーバーとブラウザ間の通信を暗号化するための技術がSSL/TLSという技術になります。

まともなサイトであればユーザーに個人情報を入力させたり機密情報を表示するようなURLは`http://`ではなく、
`https://`で始まるURLになっているはずです。

`https://`で始まるURLへアクセスしている時の通信は
SSLプロトコルを使用しており、通信内容が暗号化されます。

今回は以前作成したWebページにSSLで接続できるようにしてみましょう。
設定方法は[Comodoのオフィシャルサイト](http://comodo.jp/support/crt/dtl_50)に載っていますが、
まずは証明書の発行が必要です。

英語のサイトになりますが、下記のページからトライアル版の証明書を入手して設定してみましょう。

https://www.comodo.com/e-commerce/ssl-certificates/free-ssl-certificate.php
