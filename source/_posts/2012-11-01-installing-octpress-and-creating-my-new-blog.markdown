---
layout: post
title: "Octpressのインストール失敗と成功"
date: 2012-11-01 23:46
comments: true
categories: octpress
---

Mac OSX 10.7.5の環境です。
散々失敗した記録なので、決してまねしないでね。

{% codeblock %}
ruby --version
# => ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.3.0]
{% endcodeblock %}

1.9.3以上であることを確認。パッチのバージョンは、まあ大丈夫からと思って放置。

{% codeblock %}
git clone git://github.com/imathis/octopress.git octopress
{% endcodeblock%}

最後に設定した"octpress"の部分は、myblogとか好きな名前で大丈夫。

{% codeblock %}
cd octopress/
Do you wish to trust this .rvmrc file? (/Users/sjun/octopress/.rvmrc)
y[es], n[o], v[iew], c[ancel]> y
ruby-1.9.3-p286 is not installed.
To install do: 'rvm install ruby-1.9.3-p286'
{% endcodeblock %}

あれ、怒られた。

{% codeblock %}
ruby -v
# => ruby 1.8.7 (2012-02-08 patchlevel 358) [universal-darwin11.0]
{% endcodeblock %}

???
どうやら、rvmのrubyじゃなくて、はじめからインストールされてるやつにPATHが通ってる。
とりあえず、rvmでちゃんとインストールされてるかだけ確認。

{% codeblock %}
$ rvm list

=* ruby-1.9.3-p194 [ x86_64 ]

# => - current
# =* - current && default
#  * - default
{% endcodeblock %}

あきらかによろしくないけど、無理矢理進める。

{% codeblock %}
$ bundle install
error loading "/Users/sjun/.rvm/gems/ruby-1.9.3-p194@global/gems/rubygems-bundler-0.9.2/lib/rubygems_plugin.rb":

Older rubygems-bundler found, please uninstall it with:

    GEM_HOME="/Users/sjun/.rvm/gems/ruby-1.9.3-p194@global" gem uninstall -ax rubygems-bundler -v 0.9.2

     (RuntimeError)
     Fetching gem metadata from http://rubygems.org/.
     Error Bundler::HTTPError during request to dependency API
     Fetching full source index from http://rubygems.org/
     ^C
     Quitting...
{% endcodeblock %}

なんか怒られたw

{% codeblock %}
$ GEM_HOME="/Users/sjun/.rvm/gems/ruby-1.9.3-p194@global" gem uninstall -ax rubygems-bundler -v 0.9.2
error loading "/Users/sjun/.rvm/gems/ruby-1.9.3-p194@global/gems/rubygems-bundler-0.9.2/lib/rubygems_plugin.rb":

Older rubygems-bundler found, please uninstall it with:

    gem uninstall -ax rubygems-bundler -v 0.9.2

     (RuntimeError)
     Removing noexec
     Successfully uninstalled rubygems-bundler-0.9.2

{% endcodeblock %}

とりあえず、怒られてるgemを削除
気をとりなおして、bundlerをインストール。

{% codeblock %}
$ gem install bundler

Successfully installed bundler-1.2.1
1 gem installed
ERROR:  While executing gem ... (ArgumentError)
    undefined class/module Encoding
{% endcodeblock %}

なんかもう諦めたくなってきた。

{% codeblock %}
$ gem update
Updating installed gems
Updating gorillib
Successfully installed configliere-0.4.17
Successfully installed gorillib-0.4.2
Updating net-ssh
Successfully installed net-ssh-2.6.1
Gems updated: configliere, gorillib, net-ssh
ERROR:  While executing gem ... (ArgumentError)
    undefined class/module Encoding
{% endcodeblock %}

エラー、それでも無理矢理すすめる。

{% codeblock %}
$ bundle install
Fetching gem metadata from http://rubygems.org/.
Error Bundler::HTTPError during request to dependency API
Fetching full source index from http://rubygems.org/
Using rake (0.9.2.2)
Installing RedCloth (4.2.9) with native extensions
Installing posix-spawn (0.3.6) with native extensions
Installing albino (1.3.3)
Installing blankslate (2.1.2.4)
Installing chunky_png (1.2.5)
Installing fast-stemmer (1.0.1) with native extensions
Installing classifier (1.3.3)
Installing fssm (0.2.9)
Installing sass (3.1.20)
Installing compass (0.12.2)
Installing directory_watcher (1.4.1)
Installing ffi (1.0.11) with native extensions
Installing haml (3.1.6)
Installing kramdown (0.13.7)
Installing liquid (2.3.0)
Gem::InstallError: liquid requires RubyGems version >= 1.3.7. Try 'gem update --system' to update RubyGems itself.
An error occurred while installing liquid (2.3.0), and Bundler cannot continue.
Make sure that `gem install liquid -v '2.3.0'` succeeds before bundling.
{% endcodeblock %}

この辺りで、gemとかrvmを全くアップデートしてないことに気づく。

{% codeblock %}
gem update --system
Updating RubyGems
Updating rubygems-update
Successfully installed rubygems-update-1.8.24
Updating RubyGems to 1.8.24
Installing RubyGems 1.8.24
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied - /Library/Ruby/Site/1.8/gauntlet_rubygems.rb    
{%endcodeblock%}

このあたりで、sudoしてないせいかもしれないと思いはじめる。

{% codeblock %}
sudo gem update --system
Password:
Updating RubyGems
Updating rubygems-update
Successfully installed rubygems-update-1.8.24
Updating RubyGems to 1.8.24
Installing RubyGems 1.8.24
RubyGems 1.8.24 installed

== 1.8.24 / 2012-04-27

* 1 bug fix:

  * Install the .pem files properly. Fixes #320
    * Remove OpenSSL dependency from the http code path


    ------------------------------------------------------------------------------

    RubyGems installed the following executables:
         /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/gem

{% endcodeblock%}

お。通った。

{% codeblock %}
bundle install
Fetching gem metadata from http://rubygems.org/.
Error Bundler::HTTPError during request to dependency API
Fetching full source index from http://rubygems.org/
.
.
(略)
.
.

Using bundler (1.2.1)Your bundle is complete! Use `bundle show [gemname]` to see where a bundled gem is installed.
{% endcodeblock %}

{% codeblock %}
$ gem update
.
.
.
ERROR:  While executing gem ... (ArgumentError)
    undefined class/module Encoding
{% endcodeblock %}

まだダメか。

{% codeblock %}
sudo gem update
Updating installed gems
Updating actionmailer
Fetching: activesupport-3.2.8.gem (100%)
.
.
(以下大量のパッケージがインストールされる)
.
.
Building native extensions.  This could take a while…
(時間かかり過ぎ)
.
.
{% endcodeblock %}

パッケージとRDocが大量にインストールされて驚いたけど、
無事終わった!!

しかし、心配になってもう一度やる。

{% codeblock %}
$ sudo gem update
Password:
Updating installed gems
Nothing to update
{% endcodeblock %}

これは期待 :3 :3 :3

{% codeblock %}
$ bundle install
.
.
.
Your bundle is complete! Use `bundle show [gemname]` to see where a bundled gem is installed.
{% endcodeblock %}

よし :)

{% codeblock %}
$ rake install
## Copying classic theme into ./source and ./sass
mkdir -p source
cp -r .themes/classic/source/. source
mkdir -p sass
cp -r .themes/classic/sass/. sass
mkdir -p source/_posts
mkdir -p public
23:05:37 ~/octopress
{% endcodeblock %}

完了した！
が、このあとちゃんと動かない...
諦めて、rubyを更新、rm -rfして、再度git cloneしてくる。

{% codeblock %}
rake generate
## Generating Site with Jekyll
directory source/stylesheets/
   create source/stylesheets/screen.css
   Configuration from /Users/sjun/octopress/_config.yml
   /Users/sjun/octopress/plugins/category_generator.rb:109: warning: regexp has invalid interval
   /Users/sjun/octopress/plugins/category_generator.rb:109: warning: regexp has `' without escape
   /Users/sjun/octopress/plugins/category_generator.rb:146: warning: regexp has invalid interval
   /Users/sjun/octopress/plugins/category_generator.rb:146: warning: regexp has `' without escape
   /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/../lib/jekyll/site.rb:76:in `require': /Users/sjun/octopress/plugins/image_tag.rb:27: undefined (?...) sequence: /(?<class>\S.*\s+)?(?<src>(?:https?:\/\/|\/|\S+\/)\S+)(?:\s+(?<width>\d+))?(?:\s+(?<height>\d+))?(?<title>\s+.+)?/ (SyntaxError)
   /Users/sjun/octopress/plugins/image_tag.rb:29: undefined (?...) sequence: /(?:"|''")(?<title>[^"''"]+)?(?:"|''")\s+(?:"|''")(?<alt>[^"''"]+)?(?:"|''")/
        from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/../lib/jekyll/site.rb:76:in `setup'
             from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/../lib/jekyll/site.rb:75:in `each'
                  from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/../lib/jekyll/site.rb:75:in `setup'
                       from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/../lib/jekyll/site.rb:30:in `initialize'
                            from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/jekyll:224:in `new'
                                 from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/gems/jekyll-0.11.2/bin/jekyll:224
                                      from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/bin/jekyll:23:in `load'
                                           from /Users/sjun/.rvm/gems/ruby-1.9.3-p194/bin/jekyll:23

{% endcodeblock %}

あれ？やばいかも...

{% codeblock %}
$ sudo rake generate
Could not find RedCloth-4.2.9 in any of the sources
Run `bundle install` to install missing gems.
{% endcodeblock%}

んん？

{% codeblock %}
sudo gem install RedCloth
Fetching: RedCloth-4.2.9.gem (100%)
Building native extensions.  This could take a while...
Successfully installed RedCloth-4.2.9
1 gem installed
Installing ri documentation for RedCloth-4.2.9...
Installing RDoc documentation for RedCloth-4.2.9...
{% endcodeblock %}

{% codeblock %}
sudo rake generate
Could not find posix-spawn-0.3.6 in any of the sources
Run `bundle install` to install missing gems.
{%endcodeblock%}

こいつもインストールした。

{% codeblock %}
$ rake generate
rake aborted!
### You haven't set anything up yet. First run `rake install` to set up an Octopress theme.
{% endcodeblock%}

{% codeblock %}
rake install
rake generate
rake deploy
rake setup_github_pages
add .
git push origin source
{% endcodeblock %}

多少記憶と記録があやふや。

とりあえず、versionはあわせることとupdateを先にすることを身にしみて理解した。


-----
####参考にしたもの
* [http://stackoverflow.com/questions/11488086/ruby-could-not-find-redcloth-4-2-9](http://stackoverflow.com/questions/11488086/ruby-could-not-find-redcloth-4-2-9)
* [http://ojima-h.github.com/blog/2012/04/15/github-pages-and-octpress/](http://ojima-h.github.com/blog/2012/04/15/github-pages-and-octpress/)
* [http://cat.hackingisbelieving.org/blog/2012/08/14/how-to-install-octopress-on-your-github-pages/](http://cat.hackingisbelieving.org/blog/2012/08/14/how-to-install-octopress-on-your-github-pages/)
* [http://gam0022.github.com/blog/2012/07/23/run-octpress-on-a-vps-using-rsync/](http://gam0022.github.com/blog/2012/07/23/run-octpress-on-a-vps-using-rsync/)
* [http://octopress.org/docs/setup/](http://octopress.org/docs/setup/)

----
