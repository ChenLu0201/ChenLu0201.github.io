---
layout: post
title: "Git中的存储对象"
date: 2015-01-25 14:49:06 +0800
comments: true
author: Lane
categories: [git, git base, git objects]
---

2005年，由于linux内核开发团队当时使用的BitKeeper VCS不能满足需要，Linus希望找到一个VCS能满足足以下需求：

- Facilitate Distributed Development
- Scale to Handle Thousands of Developers
- Perform Quickly and Eficiently
- Maintain Integrity and Trust
- Enforce Accoutablility
- Immutability
- Atomic Transactions
- Support and Encourage Branched Development
- Complete Repositories
- A Clean Internal Design
- Be Free

遗憾找不到合适可用的VCS。最后开发团队决定开发自己的VCS系统，也就是后来的Git。 本篇接下来将介绍使Git具有以上特性的基础 -- Git的存储对象模型。

##Git仓库（Git Repository）
Git仓库是一个记录和管理工程版本和历史的数据仓库。Git与其它VCSs的区别在于它不仅有工作目录所有内容的拷贝，还有使Git工作的仓库拷贝。同时还对每个站点，每个用户，每个仓库维护了一组配置信息。Git仓库维护了两种基础的数据结构：对象存储和索引，所有的文件都存储在工作目录的跟路径的.git目录下。Git的对象存储经过了精心的设计，能支持分布式的高效拷贝；Git索引用来存储每个本地仓库自己的临时数据，可以根据用户的需要灵活的更改。

##Git对象类型
- Blobs

一个Blob对象代表了一个文件某个版本的内容。在计算中，我们需要引用到一些变量和文件，但是又不关心内部的具体数据时，我们经常用`Blob`[binary large object]来存储数据。Blob数据只存储文件的内容，而不存储任何文件相关的元数据，包括文件名。

- Trees

一个Tree对象代表了一层目录的信息。它记录了blob对象的标识，路径名和当前目录的一些元数据。它同时可以递归的应用其他的Subtree对象。因此它维护了完成的文件和路径的完整结构。

- Commits

一个Commit对象存储了对仓库引入的一次修改的元数据，它包括作者，提交人，提交日期，日志信息。每个Commit对象指向了包含工作仓库最新状态完整快照的Tree对象。最初的提交没有父节点，大部分提交都有一个父节点，也有存在两个父节点的情况（比如在merge的情况下）。

- Tags

一个Tag用来给其它对象指定一个可读的名字，通常是一个Commit。虽然`9da581ae67f765c8aac675de87f54a3c5ef6da`引用了一个具体的Commit,使用一个更可读的Tag,例如Ver-1.0-Alpha，会更方便。

##Index
Index是用来描述整个仓库目录结构的，临时、动态的二进制文件。更具体一点讲，Index包含某一时刻整个工程全部结构。整个工程的状态可以由工程历史中任意时间的一个Commit对象和一个Tree对象来获取。Git区别于其它VCS的一个核心特性是它提供了设计良好的方法来修改Index。Index同时能将增量开发和提交的内容区分开来。一般，开发人员会频繁的修改、删除、回退文件，Index用来记录那些已经修改完毕，并可以用于提交的内容，同时也可以从Index中取消修改。被Index记录的文件，才会被提交到远程仓库。

接下来，我们来通过一个例子来看看git的对象是怎么工作的。在我们的根路径下有两个文件，我们进行第一次提交，并为第一次提交指定一个tag。git对象的结构将会按下图的结构进行组织:

{% img /images/post20150125/obj-tree-1.jpg 'objects structure after initial commit' 1024 652 %}

在此基础上，我们在根路径下新建一个目录， 并在该目录下添加一个文件，然后，我们提交修改。Git仓库的对象结构将更新为如下图所示的结构：

{% img /images/post20150125/obj-tree-2.jpg 'objects structure after updating' 987 1022 %}

通过上面的例子，我相信大家对git中的四种对象有了更好地认识。更多Git内容将在以后的blog中继续介绍。


