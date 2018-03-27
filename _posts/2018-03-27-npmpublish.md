---
layout: post
title: "npm 发布"
keywords: vue
description: vue
date: 2018-3-27 09:00:00
categories: [npm]
tags: [npm,publish]
---

1. node 安装好
2. 创建包
```text
mkdir test007
cd test007
npm init
```
根据提示输入相关信息。
3. npm adduser
根据提示输入用户名
密码 （密码必须超过7位）
邮箱
4. npm whoami
5. npm login
6. 去邮箱里面点击校验的地址登录，看到登录界面。
7. npm publish . 发布当前目录
8. 如果提示没有权限啥的，说明包名重复，换一个，如果提示邮箱验证没通过，需要先重复6的步骤。
9. 搜到自己发布的包



  