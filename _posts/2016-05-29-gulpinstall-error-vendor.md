---
layout: post
title: "gulp run error node-sass"
keywords: gulp 
description: 前端构建工具
date: 2016-05-29 08:52:36
categories: [前端构建工具]
tags: [gulp]
---

gulp 运行出错

![看图](/assets/img/node-sass-error.jpg)

这个问题应该是由于以下问题导致：

> This usually happens because your environment has changed since running npm install.
Run npm rebuild node-sass to build the binding for your current environment.

[参考](https://github.com/sass/node-sass/issues/1162)

```text
	npm rebuild node-sass
```

![gulp success](/assets/img/gulp-success.jpg)



