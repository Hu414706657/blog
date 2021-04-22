---
title: Git
date: 2019-08-16 10:46:55
tags:
---

# 概述

Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。 
Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

# Git 常用命令

|指令 | 说明 |
| ------------------------------- | ------------------------------------------------------------ |
| git init      | 初始化：创建一个git仓库，创建之后就会在当前目录生成一个.git的文件 |
| git add filename| 添加文件：把文件添加到缓冲区                             |
| git add . <br/>                git add --all | 添加所有文件到缓冲区（从目前掌握的水平看，和后面加“.”的区别在于，加all可以添加被手动删除的文件，而加“.”不行） |
| git rm filename | 删除文件 |
| git commit -m "提交的说明" | 提交：提交缓冲区的所有修改到仓库 |
| git status | 查看git库的状态，未提交的文件，分为两种，add过已经在缓冲区的，未add过的 |
| git diff filename | 比较：如果文件修改了，还没有提交，就可以比较文件修改前后的差异 |
| git log | 查看日志 |
| git reset | 版本回退：可以将当前仓库回退到历史的某个版本 |
| git reset --hard HEAD^ | 回退到上一个版本（HEAD代表当前版本，有一个^代表上一个版本，以此类推） |
| git reset --hard d7b5 | 回退到指定版本(其中d7b5是想回退的指定版本号的前几位) |
| git reflog | 查看命令历史：查看仓库的操作历史 |
| git branch | 查看分支的情况，前面带*号的就是当前分支 |
| git branch 分支名 | 创建分支 |
| git checkout 分支名 | 切换当前分支到指定分支 |
| git checkout  -b 分支名 | 创建分支并切换到创建的分支 |
| git merge 分支名 | 合并某分支的内容到当前分支 |
| git branch -d 分支名 | 删除分支 |
| git tag | 查看所有标签 |
| git show 标签名 | 查看标签的详细信息 |
| git push origin --tags | 推送所有tag |
| git push origin v1.0 | 推送某个tag |

# IDEA常用的git操作

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea-git.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idet-git2.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idat-git3.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea-git4.png)
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/idea-git5.png)
**版本回退**
![](https://414706657.oss-cn-shenzhen.aliyuncs.com/QQ%E6%88%AA%E5%9B%BE20190823162426.png)

# Git 工作流简介

## 概述

工作流有各式各样的用法，但也正因此使得在实际工作中如何上手使用增加了难度。这篇指南通过总览公司团队中最常用的几种 Git 工作流让大家可以上手使用。

在阅读的过程中请记住，本文中的几种工作流是作为方案指导而不是条例规定。在展示了各种工作流可能的用法后，你可以从不同的工作流中挑选或揉合出一个满足你自己需求的工作流

## 集中式工作流

![](https://414706657.oss-cn-shenzhen.aliyuncs.com/git-workflow-svn.png)
转到分布式版本控制系统看起来像个令人生畏的任务，但不改变已用的工作流你也可以用上 Git 带来的收益。团队可以用和 Subversion 完全不变的方式来开发项目。

但使用 Git 加强开发的工作流，Git 比 SVN 有几个优势。首先，每个开发可以有属于自己的整个工程的本地拷贝。隔离的环境让各个开发者的工作和项目的其他部分（修改）独立开来 —— 即自由地提交到自己的本地仓库，先完全忽略上游的开发，直到方便的时候再把修改反馈上去。

其次，Git 提供了强壮的分支和合并模型。不像 SVN，Git 的分支设计成可以做为一种用来在仓库之间集成代码和分享修改的『失败安全』的机制。

**工作方式**
像 Subversion 一样，集中式工作流以中央仓库作为项目所有修改的单点实体。相比 SVN 缺省的开发分支 trunk，Git 叫做 master，所有修改提交到这个分支上。该工作流只用到 master 这一个分支。

开发者开始先克隆中央仓库。在自己的项目拷贝中，像 SVN 一样的编辑文件和提交修改；但修改是存在本地的，和中央仓库是完全隔离的。开发者可以把和上游的同步延后到一个方便时间点。

要发布修改到正式项目中，开发者要把本地 master 分支的修改『推（push）』到中央仓库中。这相当于 svn commit 操作，但 push 操作会把所有还不在中央仓库的本地提交都推上去。
![](https://www.funtl.com/assets/git-workflow-svn-push-local.png)

**解决冲突**
中央仓库代表了正式项目，所以提交历史应该被尊重且是稳定不变的。如果开发者本地的提交历史和中央仓库有分歧，Git 会拒绝 push 提交否则会覆盖已经在中央库的正式提交。

![](https://www.funtl.com/assets/git-workflow-svn-managingconflicts.png)

在开发者提交自己功能修改到中央库前，需要先 fetch 在中央库的新增提交，rebase 自己提交到中央库提交历史之上。这样做的意思是在说，『我要把自己的修改加到别人已经完成的修改上。』最终的结果是一个完美的线性历史，就像以前的SVN 的工作流中一样。

如果本地修改和上游提交有冲突，Git 会暂停 rebase 过程，给你手动解决冲突的机会。Git 解决合并冲突，用和生成提交一样的 git status 和 git add 命令，很一致方便。还有一点，如果解决冲突时遇到麻烦，Git 可以很简单中止整个 rebase 操作，重来一次（或者让别人来帮助解决）。

## GitFlow 工作流

**概述**
![](https://www.funtl.com/assets/git-workflows-gitflow.png)
GitFlow 工作流定义了一个围绕项目发布的严格分支模型。虽然比功能分支工作流复杂几分，但提供了用于一个健壮的用于管理大型项目的框架。

GitFlow 工作流没有用超出功能分支工作流的概念和命令，而是为不同的分支分配一个很明确的角色，并定义分支之间如何和什么时候进行交互。除了使用功能分支，在做准备、维护和记录发布也使用各自的分支。当然你可以用上功能分支工作流所有的好处：Pull Requests、隔离实验性开发和更高效的协作。

GitFlow 工作流定义了一个围绕项目发布的严格分支模型。虽然比功能分支工作流复杂几分，但提供了用于一个健壮的用于管理大型项目的框架。

GitFlow 工作流没有用超出功能分支工作流的概念和命令，而是为不同的分支分配一个很明确的角色，并定义分支之间如何和什么时候进行交互。除了使用功能分支，在做准备、维护和记录发布也使用各自的分支。当然你可以用上功能分支工作流所有的好处：Pull Requests、隔离实验性开发和更高效的协作。

#### 工作方式

GitFlow 工作流仍然用中央仓库作为所有开发者的交互中心。和其它的工作流一样，开发者在本地工作并 push 分支到中央仓库中。

#### 历史分支

相对使用仅有的一个 master 分支，GitFlow 工作流使用2个分支来记录项目的历史。master 分支存储了正式发布的历史，而 develop 分支作为功能的集成分支。这样也方便 master 分支上的所有提交分配一个版本号。
![](https://www.funtl.com/assets/git-workflow-release-cycle-1historical.png)

剩下要说明的问题围绕着这2个分支的区别展开。

#### 功能分支

每个新功能位于一个自己的分支，这样可以 push 到中央仓库以备份和协作。但功能分支不是从 master 分支上拉出新分支，而是使用 develop 分支作为父分支。当新功能完成时，合并回 develop 分支。新功能提交应该从不直接与 master 分支交互。
![](https://www.funtl.com/assets/git-workflow-release-cycle-2feature.png)

注意，从各种含义和目的上来看，功能分支加上 develop 分支就是功能分支工作流的用法。但 GitFlow 工作流没有在这里止步。

#### 发布分支

![](https://www.funtl.com/assets/git-workflow-release-cycle-3release.png)

一旦 develop 分支上有了做一次发布（或者说快到了既定的发布日）的足够功能，就从 develop 分支上 fork 一个发布分支。新建的分支用于开始发布循环，所以从这个时间点开始之后新的功能不能再加到这个分支上 —— 这个分支只应该做 Bug 修复、文档生成和其它面向发布任务。一旦对外发布的工作都完成了，发布分支合并到 master 分支并分配一个版本号打好 Tag。另外，这些从新建发布分支以来的做的修改要合并回 develop 分支。

使用一个用于发布准备的专门分支，使得一个团队可以在完善当前的发布版本的同时，另一个团队可以继续开发下个版本的功能。这也打造定义良好的开发阶段（比如，可以很轻松地说，『这周我们要做准备发布版本 4.0』，并且在仓库的目录结构中可以实际看到）。

常用的分支约定：

用于新建发布分支的分支: develop
用于合并的分支: master
分支命名: release-* 或 release/*

#### 维护分支

![](https://www.funtl.com/assets/git-workflow-release-cycle-4maintenance.png)
维护分支或说是热修复（hotfix）分支用于生成快速给产品发布版本（production releases）打补丁，这是唯一可以直接从 master 分支 fork 出来的分支。修复完成，修改应该马上合并回 master 分支和 develop 分支（当前的发布分支），master 分支应该用新的版本号打好 Tag。

为 Bug 修复使用专门分支，让团队可以处理掉问题而不用打断其它工作或是等待下一个发布循环。你可以把维护分支想成是一个直接在 master 分支上处理的临时发布。
