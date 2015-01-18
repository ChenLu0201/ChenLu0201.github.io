---
layout: post
title: "使用Octopress创建个人blog"
date: 2015-01-18 13:02:35 +0800
comments: true
author: Lane
categories: [setup octopress]
---
##环境设置
1. 安装[Git](http://http://git-scm.com/)
2. 安装Ruby 1.9.3或者更高版本。可以使用[rbenv](http://octopress.org/docs/setup/rbenv/)或者[RVM](http://http://octopress.org/docs/setup/rvm/)安装ruby

 如果`ruby --verion`显示你的版本不是或者高于1.9.2，请参考使用[rbenv](http://octopress.org/docs/setup/rbenv/)或者[RVM](http://http://octopress.org/docs/setup/rvm/)安装Ruby
##设置Octopress
{% codeblock %}
git clone git://github.com/imathis/octopress.git octopress
cd octopress
{% endcodeblock %}

然后，安装依赖

{% codeblock %}
gem install bundler
rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
bundle install
{% endcodeblock %}

安装默认的主题
{% codeblock %}
rake install
{% endcodeblock %}