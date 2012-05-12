---
layout: post
title: "在Cygwin下配置 jekyll bootstrap"
description: ""
category: 
tags: []
---
{% include JB/setup %}

### Step by step 教你安装jekyll
---
### 解决依赖 
* 安装Cygwin自带的libiconv
* 安装ruby的依赖yaml
{% highlight bash %}
$ wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz
$ tar xf yaml-0.1.4.tar.gz
$ cd yaml-0.1.4
$ ./configure --prefix=/usr/local/yaml && make -j2 
# make install
{% endhighlight %}
* 安装Ruby 1.9.2
{% highlight bash %}
$ wget http://ruby.taobao.org/mirrors/ruby/ruby-1.9.2-p320.tar.bz2
$ tar xf ruby-1.9.2-p320.tar.bz2
$ cd ruby-1.9.2-p320
$ ./configure --prefix=/usr/local/ruby19 --with-opt-dir=/usr/local/yaml/ && make -j2 
# make install
{% endhighlight %}
* 配置PATH
{% highlight bash %}
$ echo 'PATH=/usr/local/ruby19/bin:$PATH' >>~/.bashrc
$ source ~/.bashrc
{% endhighlight %}
* 安装gem
{% highlight bash %}
$ wget http://production.cf.rubygems.org/rubygems/rubygems-1.8.24.tgz
$ tar xf rubygems-1.8.24.tgz
$ cd rubygems-1.8.24
$ ruby setup.rb
{% endhighlight %}
* 安装posix-spawn(GEM自带的0.3.6版本对Cygwin支持有点问题详见[#20](http://github.com/rtomayko/posix-spawn/pull/20)
{% highlight bash %}
$ git clone git://github.com/rtomayko/posix-spawn.git
$ cd posix-spawn
$ gem build posix-spawn.gemspec
$ gem install posix-spawn-0.3.6.gem
{% endhighlight %}
* 安装jekyll以及一些而外依赖
{% highlight bash %}
$ gem instal -V jekyll RedCloth rdiscount
{% endhighlight %}
* 修复jekyll 启动错误
如遇到 ``invalid byte sequence in GBK (ArgumentError)`` 执行 
{% highlight bash %}
echo -e 'export LC_ALL="en_US.UTF-8"\nexport LANG="en_US.UTF-8"' >>~/.bashrc
{% endhighlight %}
还不行就参考参考[这个](http://www.oschina.net/question/129471_37163)

---
### 配置blog

* 克隆jekyll-bootstrap并开启测试
{% highlight bash %}
git clone git://github.com/plusjade/jekyll-bootstrap.git djluo.github.com
cd djluo.github.com
jekyll --server
#可以用游览器访问了 http://127.0.0.1:4000/
{% endhighlight %}
<!--
 vim: expandtab tabstop=4 shiftwidth=4 softtabstop=4
-->
