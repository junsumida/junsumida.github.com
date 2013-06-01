---
layout: post
title: "Rails 3 ActiveRecordでNOT NULLする方法"
date: 2012-11-23 21:45
comments: true
categories: "rails"
---

ActiveRecordでNOT NULLを使う方法。

{% codeblock lang:ruby %}
ModelName.where("id IS NOT NULL")
{% endcodeblock %}

ただし、Squeelというライブラリを使うと、
{% codeblock lang:ruby%}
Foo.includes{bar}.where{bars.id != nil}
{% endcodeblock %}
のようにして書くことができるらしい。

----
####works cited:
* [rail 3 where condition using NOT NULL](http://stackoverflow.com/questions/10539502/selecting-distinct-where-not-null-in-mysql)

----
