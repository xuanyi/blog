---
title: ruhoh blog安装配置
date: '2013-03-03'
description:
categories:
---

### 安装Ruby

Ruby的版本要>= `1.9.2`，否则后面安装psych时会装不上。

使用`yum`安装默认装的是1.8.7。
编译安装1.9.3时出错，我已经安装过1.8.7。
最后使用RVM安装1.9.3成功。

#### rvm安装配置

1. 安装rvm
<pre><code>curl -L https://get.rvm.io | bash -s stable</code></pre>

1. 安装一些依赖库，编译安装其它软件或者库的时候会用到
<pre><code>yum install -y gcc-c++ patch readline readline-devel zlib zlib-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison iconv-devel</code></pre>

1. 安装Ruby 1.9.3
<pre><code>rvm install 1.9.3</code></pre>

1. 运行`ruby -v`
<pre><code>ruby 1.9.3p392 (2013-02-22 revision 39386) [x86_64-linux]</code></pre>

1. ok,1.9版本以上自带了gem,不需要另行安装

### 安装Ruhoh

1. 使用`gem`安装
<pre><code>gem install ruhoh</code></pre>

1. 执行`ruhoh help`，查看如何使用ruhoh新建一个blog

### 生成blog

1. `ruhoh new` 一个blog
<pre><code>ruhoh new blog</code></pre>
<pre><code>
[root@S268977 tmp]# ruhoh new blog
Trying this command:
  git clone git://github.com/ruhoh/blog.git /var/www/tmp/blog
  Initialized empty Git repository in /var/www/tmp/blog/.git/
  remote: Counting objects: 266, done.
  remote: Compressing objects: 100% (132/132), done.
  remote: Total 266 (delta 115), reused 244 (delta 100)
  Receiving objects: 100% (266/266), 86.34 KiB | 114 KiB/s, done.
  Resolving deltas: 100% (115/115), done.
  Success! Now do...
  cd /var/www/tmp/blog
  rackup -p9292
  http://localhost:9292
</code></pre>
至此一个blog就诞生了！

1. 接下来我们使用2.0.0版本，切换到2.0.alpha分支，2.0版本语法与之前有[差异](http://ruhoh.com/docs/2/)。
<pre><code>
$ cd blog
$ git checkout 2.0.alpha
</code></pre>

1. 我们可以把它放到自己github上的仓库里
<pre><code>
$ git remote rm origin
$ git remote add origin git@github.com:USERNAME/my-ruhoh-blog.git
$ git push
</code></pre>

1. 最后执行`ruhoh compile`，生成`compiled`目录，此目录里是生成生的静态文件。在Apache中新建一个站点指向`compiled`目录,就可以访问blog了。
<pre><code>ruhoh compile</code></pre>

#### 也可以使用[ruhoh官方网站](http://ruhoh.com)提供的方法，都一样。

