# server-walkthrough

サーバーを触ったことない人用のウォークスルーです。

詳細は以下のWikiをご覧ください。

https://github.com/towninc/server-walkthrough/wiki

#### 内容

- docs -> 記事の.mdファイル
- images -> `resources/images.key`から切り出した記事掲載用画像
- resources/images.key -> 記事掲載用画像の切り出し元ファイル

#### 各ページの目次自動生成・更新方法

[doctoc](https://github.com/thlorenz/doctoc) を使用しました。

```
$ npm install -g
$ make
```
