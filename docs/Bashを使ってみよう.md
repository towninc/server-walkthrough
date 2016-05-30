
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Bashとは](#bash%E3%81%A8%E3%81%AF)
  - [Bashって何？](#bash%E3%81%A3%E3%81%A6%E4%BD%95%EF%BC%9F)
  - [Bashの利点](#bash%E3%81%AE%E5%88%A9%E7%82%B9)
- [Bashを使ってみよう](#bash%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86)
  - [変数](#%E5%A4%89%E6%95%B0)
  - [制御構文](#%E5%88%B6%E5%BE%A1%E6%A7%8B%E6%96%87)
  - [リダイレクト](#%E3%83%AA%E3%83%80%E3%82%A4%E3%83%AC%E3%82%AF%E3%83%88)
  - [パイプ](#%E3%83%91%E3%82%A4%E3%83%97)
- [課題](#%E8%AA%B2%E9%A1%8C)
  - [課題1](#%E8%AA%B2%E9%A1%8C1)
  - [課題2](#%E8%AA%B2%E9%A1%8C2)
  - [応用課題](#%E5%BF%9C%E7%94%A8%E8%AA%B2%E9%A1%8C)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Bashとは
## Bashって何？

BashとはLinux系OSでは初期選択されているシェル（ユーザーインターフェースソフト）です。

![Bash](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/linux_shell.png)

Windowsでいうところのエクスプローラーみたいなもんだと考えてください。

![GUIシェル(Windowsなど)](https://raw.githubusercontent.com/towninc/server-walkthrough/master/images/gui_shell.png)


実は、いままで幾つかコマンドを実行してきましたが、それらのコマンドを渡していた相手がBashなのでした。

## Bashの利点

Bashの利点はBashスクリプトとしてサーバー作業を記述できるところです。

例えばアプリケーションサーバーとDBサーバーを停止して、ファイルを差し替えて起動するなどは

```
#!/bin/sh

service tomcat7 stop
service mysql stop

cp -rf /path/to/dir/src /path/to/dir/desc

service mysql stop
service tomcat7 start
```

と書いて、実行してやることが可能です。

つまり、「左下に表示されているマークをクリック、コンピュータの形のアイコンをクリックした後に、Cドライブという名前の銀色のアイコンをクリックして...」というような、面倒くさい操作を上記のようなテキストファイルで記述できるのです！

何度もおこなうことについてはスクリプト化しておくと再利用することができ、他の人に作業を任せるときに担当者によって作業内容が変わりづらくなるというメリットがあります。


# Bashを使ってみよう

変数や制御構文といった機能を組み合わせることで、
複雑な処理を組み立てていくことができます。

詳しいBashの仕様や使い方は以下など参考になるかと思います。

- [bash 入門](https://rat.cis.k.hosei.ac.jp/article/linux/bash_intro.html)
- [bash | 変数型と文字列、配列の取り扱いやif,for,whileなどの制御文の基本文法](http://bi.biopapyrus.net/linux/bash.html)

ターミナルに打ちこんで試してみましょう。

## 変数

計算結果などを一時的に保存しておくための入れ物として使えます。

```
# 変数定義(数値)
num=1

# 変数の内容を表示
echo $num
# => 1

# 数値計算
# $(( )) で括ります
# 参考: Bash で計算してみる
# http://www.odin.hyork.net/write/write0293.html
echo $(( $num + 2 ))
# => 3

# 変数定義(文字列)
str="sum"

# 変数や文字列、計算結果を結合
result=$str'='$(( $num1 + $num2 ))
echo $result
# => sum=3

# 変数定義（配列）
# 参考: 配列を使用する
# http://shellscript.sunone.me/array.html
arr=(1 2 "abc")

# 配列の長さ
echo ${#arr[@]}
# => 3

# 配列の全要素を列挙
echo ${arr[@]}
# => 1 2 abc

# 配列の各要素を表示
echo ${arr[0]}
# => 1
echo ${arr[1]}
# => 2
echo ${arr[2]}
# => abc
```

## 制御構文

繰り返し処理や条件分岐を仕込むことができます。

```
vim for_loop.sh
--↓からコピペ--
#!/bin/bash

arr=$(seq 1 10)

for i in $arr; do
    if `test $((i % 2)) = 0 `; then
        date;
    fi
    sleep 1;
done
--↑までコピペ--

# 実行
bash for_loop.sh
# 1秒おきに日時が表示される(5回)
火  5 24 15:29:40 JST 2016
火  5 24 15:29:42 JST 2016
火  5 24 15:29:44 JST 2016
火  5 24 15:29:46 JST 2016
火  5 24 15:29:48 JST 2016
```

if文の条件判定部分は、下記のように書くこともできます(testコマンドの省略記法)。

```
if [ $((i % 2)) = 0 ]; then
```

参考: [if 文と test コマンド](http://shellscript.sunone.me/if_and_test.html)

## リダイレクト

コマンドの出力結果をファイルに保存できます。

```
# date_test.txtに
# dateコマンドの出力結果を保存
date > date_test.txt

# date_test.txtに
# dateコマンドの出力結果を追記
date >> date_test.txt

# date_test.txt の内容表示
cat date_test.txt
# => yyyy年  ○月 ○○日 火曜日 hh:mm:ss UTC
# => yyyy年  ○月 ○○日 火曜日 hh:mm:ss UTC
```

## パイプ

前のコマンドの実行結果を次のコマンドの入力として利用する事ができます。

```
date
# => 2016年  ○月 ○○日 火曜日 05:26:16 UTC

# 参考: cut テキストを切り出す
http://x68000.q-e-d.net/~68user/unix/pickup?cut
date | cut -d ' ' -f 1
# => 2016年
```

# 課題

## 課題1

bashには、`$RANDOM`という変数が存在しており、0から32767までのランダム整数を取得することができます。

```
echo $RANDOM
# => 591
echo $RANDOM
# => 16104
...
```

この変数を使用して、0から31までの範囲の整数を50個表示するシェルスクリプトファイル(例: `random_numbers.sh`)を記述してください(ただし、1行に1つの数 => 全部で50行となるようにする)。

そのスクリプトの出力結果を適当なファイル(`random_test.txt`など)に保存しておいてください。

```
bash random_numbers.sh > random_test.txt
```

### 参考

- [bash > 乱数を得る > シェル変数RANDOMを使う](http://qiita.com/7of9/items/60b59fd872fe378726fd)
- [シェルやシェルスクリプトで乱数を使う2つの方法](http://news.mynavi.jp/articles/2010/04/30/shell-random-number/)

## 課題2

課題1の出力結果ファイルの内容を、昇順・重複なしで出力するコマンドを記述してください。

## 応用課題

とあるWebサイトのアクセスログから、アクセスの多いIPアドレスと回数を降順に並べてください。

アクセスログ(`accesslog-sample.tar.gz`)はRedmineのチケットに添付してあります。
(添付されていなければ、遠慮なく社員の方やアルバイトの方に聞いて下さい。)

Linuxのコマンドで使えそうなものを組み合わせてコマンドを作成してみてください。

リクエストが変に多いIPが見つかったら、
[こちら](http://www.aguse.jp/)でIPの国を探します。
国外なら大概アタックです。
