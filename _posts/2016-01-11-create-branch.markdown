--- 
layout: post
title: "git 创建分支命令"
date:   2016-01-11 15:12:11
keywords: git,创建分支，建立trace
description: git,创建分支，建立trace
categories: [git]
tags: [创建分支]
---

只使用命令行创建分支并在其上面工作

1. 先切换到主分支（一般从master新建分支）
2. 新建并切换到分支
3. 分支push到远程
4. 建立关联

```text
	git checkout master
	git checkout -b branchName
	git push origin branchName
	git branch --set-upstream-to=origin/0111-marketing branchName
```

这样以后就可以 pull/push 直接拉/推 了。

至于上面几个命令的理解：

```text
	git checkout -b [分支名] [远程名]/[分支名]
    git checkout -b serverfix origin/serverfix
```

另一些分支删除 创建操作

```js
    git branch -d master //删除一个分支
    
    git branch master mydev // 用后面的分支作为起点创建分支
    
    git checkout master // 切到这个分支
```

