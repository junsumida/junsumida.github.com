---
layout: post
title: "powとpowderのインストール"
date: 2013-03-02 19:22
comments: true
categories: 
---

Powが便利らしいので、導入してみました。

##Powとは

公式によると、

> PowはMac OS Xのための設定不要のRackサーバーです。Rails/Rackのアプリケーションの開発を可能な限りスムーズにします。
> (Pow is a zero-configuration Rack server for Mac OS X. It makes developing Rails and Rack applications as frictionless as possible.)
> 
> [Pow User's Manual](http://pow.cx/manual.html)

ということなのですが、

簡単に言えば、 **普段ローカルでlocalhost:3000とかで開発しているのを、http://hogehoge.dev/のようにアクセスできるようにしてくれます。**

##Powのインストール

通常であればこれでインストールできます。

{% codeblock lang:bash %}
  curl get.pow.cx | sh
{% endcodeblock %}

###tmuxを使っている場合

tmuxからインストールしようとすると、こんな感じに怒られてしまいます。

{% codeblock lang:bash %}
  *** Installing Pow 0.4.0...
  *** Installing local configuration files...
  /Users/sjun/Library/LaunchAgents/cx.pow.powd.plist
  *** Installing system configuration files as root...
  /Library/LaunchDaemons/cx.pow.firewall.plist
  *** Starting the Pow server...
  !!! Couldn't find a running Pow server on port 20559
{% endcodeblock %}

なので一度、tmuxを抜けてインストールしましょう。
githubのissueにも載っていました。
> [https://github.com/imathis/octopress/issues/117](https://github.com/imathis/octopress/issues/117)

##Powを使う

これも公式に載っていることですが、
{% codeblock lang:bash %}
  cd ~/.pow
  ln -s /path/to/myapp
{% endcodeblock %}

とだけすれば、大丈夫です。

##Powをもっと便利に(Powder)

Powだけでも十分便利ですが、Powderと呼ばれるpowの管理ツールを使うともっと簡単に、powを扱うことができます。

###Powderのインストール

{% codeblock lang:bash %}
 gem install powder
{% endcodeblock %}

> [powder@github](https://github.com/Rodreegez/powder)

###Powderを使う
{% codeblock lang:bash %}
#現在のディレクトリのアプリケーションとpowをつなぐ
powder link
Your application is now available at http://ur_app.dev/
#現在のディレクトリのアプリケーションとpowを切断する
powder unlink                                                                                                                               
Successfully removed ur_app
#アプリケーションのログを出力
powder applog
{% endcodeblock %}

とこんな感じに便利に使うことができるようになりました。
複数のアプリケーションを同時に開発してる時などに、localhost:4321みたいなのを手入力したり、補完しなくてもよいのはいいですね。
