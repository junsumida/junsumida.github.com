---
layout: post
title: "Rubyの正規表現において、URLでマッチさせる"
date: 2013-02-07 15:29
comments: true
categories: 
---

以下のようなURLがあって、

{% codeblock lang:ruby %}
	url = http://google.com/foo/bar.html
{% endcodeblock %}
それに対して、以下のURLでマッチさせたい時、
べた書きの正規表現であれば、エスケープを使って書くことができるが、

{% codeblock lang:ruby %}
	#マッチさせたいURL
	http://google.com/
	#べた書き正規表現
	url.match(/http:\/\/google\.com\//)

	#このように書くことができる
	Regexp.new(Regexp.quote('http://google.com/'))
{% endcodeblock %}

公式ドキュメントによれば、

> 正規表現において特別な意味を持つ文字の直前にエスケープ文字(バックスラッシュ)を挿入した文字列を返します。

とのことなので、引き渡した文字列を自動的にエスケープしてくれるようだ。

これを使うと、変数に入ったURLを正規表現のマッチャーとして使うときに、非常に便利になる。具体的には、

{% codeblock lang:ruby %}
	urls = ["http://yahoo.com/", "http://google.com/"]
	urls.each do |url|
		if long_url.match(Regexp.new(Regexp.quote(url)))
			do_something
		end
	end
{% endcodeblock %}

のようにリストを渡して、沢山処理する必要があるときに使うと、
とても便利に使うことができる。

#### 参考文献

* [Another way instead of escaping regex patterns?](http://stackoverflow.com/questions/3518161/another-way-instead-of-escaping-regex-patterns)
* [Regexp - Rubyリファレンスマニュアル](http://www.ruby-lang.org/ja/old-man/html/Regexp.html)
* [=~ (String) - Rubyリファレンス](http://ref.xaio.jp/ruby/classes/string/match_operator) 
