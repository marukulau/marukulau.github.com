---
layout: post
title: "gem install 出现错误 cannot load such file -- openssl"
date: 2014-05-14 22:14:53 +0800
comments: true
categories: 
---

今天想要在cocoapods加入一个库，用`pod install`后出现这个错误，百思不得其解，上网找了一圈发现，这个问题应该是更新了Mac OS X导致的，解决方法如下：

* brew install openssl

* rvm pkg install openssl

* rvm reinstall [version]

以上命令执行一遍，问题解决。

