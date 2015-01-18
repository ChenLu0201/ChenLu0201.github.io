---
layout: post
title: "使用Octopress创建个人blog"
date: 2015-01-18 13:02:35 +0800
comments: true
author: Lane
categories: [setup octopress]
---

[Octopress](http://octopress.org/)是基于Jekyll的静态博客引擎，并支持将博客发布到[Github Pages](https://pages.github.com/), Heroku和Rsync. 本文将指导大家使用Octopress搭建属于你自己的博客系统，并发布到[Github Pages](https://pages.github.com/)上。

##环境设置
1. 安装[Git](http://http://git-scm.com/)
2. 安装Ruby 1.9.3或者更高版本。可以使用[rbenv](http://octopress.org/docs/setup/rbenv/)或者[RVM](http://http://octopress.org/docs/setup/rvm/)安装ruby。如果`ruby --verion`显示你的版本不是或者高于1.9.2，请参考使用[rbenv](http://octopress.org/docs/setup/rbenv/)或者[RVM](http://http://octopress.org/docs/setup/rvm/)安装Ruby
3. 下载Octopress
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
##部署blog到Github pages上

这里将使用Github User/Organization pages来部署blog. 首先创建一个新的Github仓库，仓库的命名格式是`username.github.io`。`username`是你的Github账户名。创建好仓库后，我们在上面克隆的octopress目录下执行
{% codeblock %}
rake setup_github_pages
{% endcodeblock %}

这个任务将提示你输入刚才的仓库地址。它帮你完成了以下工作：

1. 提示用户输入并存储远程仓库路径。
2. 将本地仓库的git remote做调整，imathis/octopress将有origin变为octopress.
3. 将输入的仓库地址设置为defalut origin remote.
4. 将本地分支从master切换到source上。
5. 根据仓库地址配置blog url。
6. 在_deploy目录下配置部署用的master分支。

接下来执行
{% codeblock %}
rake generate
rake deploy
{% endcodeblock %}
将为你生成blog的静态页面，并拷贝到_deploy目录下， 同时部署到仓库的master分支上。当提交完你的blog site后， 请不要忘记将blog的源码提交到仓库，做好备份。
{% codeblock %}
git add .
git commit -m "your message"
git push origin source
{% endcodeblock %}

##写一篇新博客
octopress提供了一个rake task为你按照约定的命名规则创建一篇blog post.
{% codeblock %}
rake new_post["title"]
{% endcodeblock %}
`new_post`需要指定post的标题，缺省的post模板类型是`markdown`.

**Example:**

{% codeblock %}
rake new_post["my new post"]
# Creates source/_posts/2015-01-18-my-new-post.markdown
{% endcodeblock %}

如果你已经编辑好了你的post，接下来执行rake generate; rake deploy之后，访问`https://username.github.io`,你将看到自己的blog主页。
到此，你就掌握了怎样使用octopress搭建自己的blog系统。 当然让后续还有很多工作要做，比如配置个性化的域名，设计个性化的主题，为blog添加评论系统等。后续将逐渐补充。