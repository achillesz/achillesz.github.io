---
layout: post
title: "webstorm git 突然不能使用了"
keywords: git webstorm
description: mac 下webstorm不能使用了
date: 2016-10-13 10:31:36
categories: [工具]
tags: [git, webstorm]
---

mac 下 webstorm 突然不能使用了,主要表现在,文件颜色的记录上不准确,还有任何通过编辑器查看GIT相关操作,都不行了。

发现提示如下: Agreeing to the Xcode/iOS license requires admin privileges, please re-run as root via sudo

[查阅资料](http://stackoverflow.com/questions/26197347/agreeing-to-the-xcode-ios-license-requires-admin-privileges-please-re-run-as-r) 
 
 
 尝试: 
 
 ```text
  sudo xcodebuild -license
  Password:
  You have not agreed to the Xcode license agreements. You must agree to both license agreements below in order to use Xcode. 
  Hit the Enter key to view the license agreements at '/Applications/Xcode.app/Contents/Resources/English.lproj/License.rtf'
 ```
 
 按照上面路径打开程序,按照操作成功了。
 
 

  