---
layout: post
title: "Git diff介绍"
date: 2015-02-02 00:14:54 +0800
comments: true
author: Lane
categories: [Git, Git base, Git diff]
---

在我们使用VCS时，相信大家最常使用的应该是diff命令了。我们需要比较当前修改了什么，某次提交修改了什么，不同提交之间的差异是什么等各种比较。在使用Git时，也不例外。本文将介绍在Git中怎样使用Git diff，以及它的一些特殊使用方法。

##Git diff命令的几种形式
- git diff

  比较工作路径和索引中文件的区别。他将显示工作路径下需要添加至索引中的内容。

- git diff commit

  比较工作路径和指定提交之间的区别。

- git diff -\\-cached commit

  比较添加至索引的内容和指定提交之间的区别。

- git diff commit commit

  比较任意指定的两个提交之间的区别。

*通过下图我们将对以上命令的使用有更直观的了解：*

{% img center /images/post20150202/git-diff.jpg 'git diff commands' %}


##git diff使用的参数
- --M

  只检测重命名的文件。
- -w or --ignore-all-space

  忽略空白字符比较。
- --stat

  在比较结果中添加统计信息，例如修改行数、添加行数、删除行数。
- --color

  对diff结果进行着色，不同类型修改会用不同的颜色来显示。

##git diff与git log的区别
`git diff branchA branchB`和`git diff branchA..brancheB`等价，比较两个分支之间的差异。`git log branchA..branchB`是显示branchB中存在，branchA中不存在的提交。

##使用相对提交名
Git中，除了初始提交，其它的提交节点都有至少一个父亲节点。Git使用符号^用来表示父亲节点，使用符号~来表示前一个提交节点。例如，我们有名为C的分支，例如我们可以按以下规则来使用相对提交名：

{% img center /images/post20150202/relative-commit-1.jpg 800 600 'git relative commit' %}

提交C有三个父节点,可以按如下方式来访问C\^1, C\^2, C\^3.

{% img center /images/post20150202/relative-commit-2.jpg 800 600 'git relative commit' %}

提交C的第一个父节点是C~1, 第二个父节点是C~2, 第三个父节点是C~3.


我们可以使用`git diff --state master~5 master`来得到master分支当前与之前的第五次提交之间的diff统计数据。以上就是git diff命令使用的简要介绍。

