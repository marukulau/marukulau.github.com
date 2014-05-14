---
layout: post
title: "Autolayout使用笔记"
date: 2014-05-14 21:52:08 +0800
comments: true
categories: [iOS]
---

iOS6开始就引入了autolayout特性，使用autolayout进行自动布局确实方便了很多，下面是autolayout使用的一些心得。


每次设置完Label的text属性后，需要使用

``` objc
[self setNeedsUpdateConstraints];
[self updateConstraintsIfNeeded];
```


这两个方法进行更新布局，接着使用

```
[self setNeedsLayout];
[self layoutIfNeeded];
```


更新控件的frame等属性。


使用以下方法进行计算当前view的最小size：

```
CGFloat height = [self systemLayoutSizeFittingSize:UILayoutFittingCompressedSize].height;
```


计算结束后记得设置当前view的实际高度：

```
self.height = height;
```
