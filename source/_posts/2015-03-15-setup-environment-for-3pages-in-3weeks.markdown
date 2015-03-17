---
layout: post
title: "三周三页面环境搭建指南"
date: 2015-03-15 12:33:51 +0800
comments: true
categories: [3 pages in 3 weeks, setup environment]
---

   这次三周三页面workshop的培训内容是怎样设计和实现一个美观的web page，其中涉及到html和css的内容。作为基础，我们首先需要有一个快速的开发环境，本文将指导大家搭建一个高效的前端页面开发环境和安装一组实用的提升前端开发效率的工具。

##环境准备
- Ruby安装

  开发环境是一个基于Ruby的工程，所以需要先安装Ruby. 由于Mac OS自带的ruby不方便管理，建议使用`rvm`([怎么安装rvm](https://rvm.io/rvm/install))来管理你的`ruby`([怎么安装ruby](https://rvm.io/rubies/installing))。

- 准备开发工程

  开发的[模板工程](https://github.com/ChenLu0201/static-pages-boilerplate.git)已经为大家准备好。这个`Ruby`工程使用基于sass的compass库。同时使用guard来实时编译工程，使我们能及时的看到最新的修改。Fork工程后，clone到本地之后，在根路径下执行`bundle install`来执行一些初始化操作。

##工具安装

- sublime编辑器

   [Sublime](http://www.sublimetext.com/3)是一个非常好用的，现代的，跨平台的编辑器，你可以在Windows，Linux以及Mac OSX上使用它。虽然它不是免费的，但是如果你不购买，功能上没有任何的限制（除了不定时的弹出一个对话框外）。同时，它非常易于扩展。
   当你下载并安装完sublime之后，我们首先需要安装[Packge Control](https://sublime.wbond.net/installation)来管理其它插件。安装了Package Control之后，我们来安装以下插件：

   1. [Emmet](http://emmet.io/)(The Essential Toolkit for Web-developers)
   2. [HTML-CSS-JS Prettify](https://github.com/victorporof/Sublime-HTMLPrettify)(HTML/CSS/JS Formatter)
   3. [GitGutter](http://www.jisaacks.com/gitgutter)（在sublime的编辑窗口显示文本的git状态）


   安装插件非常容易，`command + shift + p`打开命令行窗口，输入`install package`:

{% img center /images/post20150315/command+shift+p.jpg 800 600 'open cmd on sublime' %}

   然后输入需要安装的插件名称:

{% img center /images/post20150315/install-package.jpg 800 600 'install package' %}


- LiveReload

  这是一个浏览器插件，能实时的刷新页面来查看修改。在chrome上，我们可以在extensions中搜索并安装它:

{% img center /images/post20150315/install-livereload.jpg 800 600 'install liveReload' %}

完成以上步骤，大家的开发环境也就搭建好了，现在我们可以开动起来实现自己的页面了。