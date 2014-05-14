---
layout: post
title: "About system volume progress view rotation issue"
date: 2014-05-14 22:10:14 +0800
comments: true
categories: [iOS] 
---


使用MPVolumeView时 system volume progress view 则不会出现， 不过如果有时想同时出现自己定制的volume bar和 system volume progress view时，这时就不能使用MPVolumeView了，需要自己使用UISlider自定义UI和关联逻辑进行实现。

然而，如果app设定为只是Landscape方向的话，system volume progress view可能就回发生rotation的问题。例如在竖屏的情况下打开app， app自动旋转到landscape方向展示，但是这时使用设备的音量调节按钮进行调节声音，可能会发现system volume progress view显示的方向还是处于竖屏的方向，没有自动旋转为横屏，解决方法如下：

由于app里面虽说大部分只支持横屏，但是，也有需要支持竖屏的情况，如查看pdf。
于是需要在AppDelegate里加入方法：

``` objc
- (NSUInteger)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window

{

    return UIInterfaceOrientationMaskAll;


}
```

支持所有方向，具体支持的方向有ViewController里面的autoRotation方法返回的值决定。


修改xxx-Info.plist文件的Supported interface orientations (iPad) 项，去掉竖直方向的两个参数。只剩下landscape两个方向。


至此问题彻底解决。
