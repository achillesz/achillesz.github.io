---
layout: post
title: "Ubuntu node 安装"
keywords: node 安装 
description: node 安装 
date: 2016-05-06 08:52:36
categories: [linux]
tags: [node]
---

解压完 tar 包之后， `bin` 目录下有 `node` `npm`。

./node -v 成功执行。

要在全局生效：

```text
	 ln -s /home/achilles/Downloads/node-v4.4.3-linux-x64/bin/npm /usr/local/bin/npm
	 ln -s /home/achilles/Downloads/node-v4.4.3-linux-x64/bin/node /usr/local/bin/node
```
