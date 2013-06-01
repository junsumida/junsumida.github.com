---
layout: post
title: "FedoraでUSB HDDをマウント"
date: 2013-03-02 21:53
comments: true
categories:linux
---

ホームサーバーとしてFedoraを使っているのですが、サーバーとしてインストールするとデフォルトでは自動でマウントしてくれないので、自分でマウントする必要があります。

{% codeblock lang:bash %}
#mountしたいデバイスがどこに繋がっているかを確認するため、syslog daemonが出力しているログを参照します
tail -f /var/log/messages

#sdb1と表示されていたら、
mount /media/usb-hdd /dev/sdb1
{% endcodeblock %}

> [dmesg と/var/log/messages の違いは](http://www.tooljp.com/linux/faq/1285E01679D0B05549257A1A0053BEC4.html)
> [ハードディスク増設方法を教えて下さい](http://oshiete.goo.ne.jp/qa/740165.html)
