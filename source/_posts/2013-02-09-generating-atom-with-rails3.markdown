---
layout: post
title: "Rails3でAtom Feedを生成"
date: 2013-02-09 17:37
comments: true
categories: 
---

はじめに
------

Builderとは
----------

>Buidlerテンプレートは、erbのようなテンプレートの一種ですが、XMLコンテンツの生成に特化しています。拡張子が.builderのテンプレートは自動的にxmlというオブジェクトを持ちます。
> [ActionView::Base](http://api.rubyonrails.org/classes/ActionView/Base.html)

#### 参考文献

FeedのURLにアクセスすると 406 Not Acceptableが返ってくる
----------------------------

FeedのURLにアクセスするとこんな感じで406が帰ってきてしまいまいた。原因は、respond_toで指定されてないformatをリクエスト指定していたからでした。

{% img ./images/406_not_acceptable.jpg %}


[respond_toで指定されていないformatは406 Not Acceptableが返ってくる](http://d.hatena.ne.jp/h-yano/20080217/1203243428)

Feed Validator
--------------

Feedが仕様通りに生成されているかを確認するには、Feed Validatorを使います。仕様通りに生成されていないと、Google Webmaster ToolsにFeedをサイトマップとして登録した際に怒られてしまいます。私は[Feed Validator](http://feedvalidator.org/check.cgi)を使いました。


