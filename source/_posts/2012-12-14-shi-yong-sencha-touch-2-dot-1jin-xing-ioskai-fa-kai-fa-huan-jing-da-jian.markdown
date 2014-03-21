---
layout: post
title: "使用Sencha Touch 2.1进行iOS开发 —— 开发环境搭建"
date: 2012-12-14 15:36
comments: true
categories: [Sencha Touch] 
---


最近由于要用到Sencha Touch 2.1进行项目的开发，于是开始了对Sencha Touch的学习，学习Sencha Touch第一步当然是搭建开发环境。  

##开发环境搭建
###Sencha iOS开发基本环境
* Mac OSX
* Xcode
* Sencha Touch SDK
<!-- more -->
###Sencha Touch SDK下载
开发环境搭建的第一步当然就是下载SDK，Sencha Touch 2.1的SDK可以到官网下载：[下载页面](http://www.sencha.com/products/touch/download/), 这里需要填个邮箱地址，随便填一个地址就可以了，会自动跳转到下载页面。  

###创建Sencha iOS项目
SDk下载好了之后就可以通过PhoneGap或者直接自己建个项目在webView里面加载Sencha页面就可以了。事实上还有第三种方法，就是Sencha Cmd，官方对Sencha Cmd的介绍是：
>Sencha Cmd is a cross-platform command line tool that provides many automated tasks around the full life-cycle of your applications, from generating a new project to deploying an application to production.  

也就是说，我们可以直接使用Sencha Cmd这个工具进行全生命周期的应用开发。另外，官网还有个Sencha SDK Tools，不过官网也建议：
>Please note: Sencha SDK Tools are designed to be used with Sencha Touch 2 and Ext JS 4.0 and are deprecated for the most current framework releases. For the current version of Sencha’s build tools, please refer to Sencha Cmd, our unified tool for Sencha’s JavaScript frameworks.

所以还是老老实实下载Sencha Cmd吧，Sencha Cmd [下载地址](http://www.sencha.com/products/sencha-cmd/download)。  

下载后安装好就可以打开终端敲命令进行创建Sencha项目了，我们把项目创建在桌面，项目名称为HelloSencha，命令如下：  

	$ sencha generate app HelloSenCha ~/Desktop/HelloSenCha
	
	Sencha Cmd v3.0.0.250
	[INF]		init-properties:
    [INF]		init-sencha-command:
    [INF]		init:
    [INF]		-before-generate-workspace:
    [INF]		generate-workspace-impl:
    [WRN]		Ignoring @require ../version/Version.js in js/String.js
    [WRN]		Ignoring @require ../Ext-more.js in js/Format.js
    [INF]		-before-copy-framework-to-workspace:
    [INF]		copy-framework-to-workspace-impl:
    
等命令执行完毕后，到桌面就可以看到创建好的项目了。

###编译项目
无需通过Xcode，我们就可以直接执行命令进行编译，编译结果将放在../build 文件夹下。

 	$ sencha package build ~/Desktop/HelloSenCha/packager.json

###运行项目
同样，我们无需打开Xcode，直接通过命令就可以把我们刚才创建的项目在iOS模拟器里面运行起来：

	$ sencha package run ~/Desktop/HelloSenCha/packager.json

现在，我们已经知道了怎么创建、编译和部署项目了，接下来就可以进行Sencha Touch SDK的学习了。
