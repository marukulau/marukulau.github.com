---
layout: post
title: "横屏竖屏自适应总结"
date: 2014-03-24 11:47:25 +0800
comments: true
categories: [Object C, iOS] 
---

所有frame的高度和宽度应该通过superview的bounds计算。
xib中的view无法设置auto mask的必须通过代码设，不设定的话有时可以自动适应，但是有时会出现有部分黑屏的情况。
两边都不设置mask则为居中显示。

以下两方法为rotate是自动调用，如果该viewController没有navigationController时，以下两方法可能不被调用，需要自己加入通知中心。
``` objc
- (void)didRotateFromInterfaceOrientation:(UIInterfaceOrientation)fromInterfaceOrientation
```
调用此方法时superview.bounds已经改变

<!-- more -->
```
- (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
```
调用此方法时superview.bounds未改变



获取当前屏幕方向
```
UIInterfaceOrientation currentOrient = [UIApplication  sharedApplication].statusBarOrientation;
```


判断当前设备是否为4寸屏
```
#define isIPhone5 ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(640, 1136), [[UIScreen mainScreen] currentMode].size) : NO)
```


3.5与4寸屏高度相差88.f,宽度一样为320.f
自适应横屏一般修改automask的autowidth,导航栏和一般控件主要变化的是宽度，高度也变化的一般是可以tableView和scrollView等。


有时候横屏没有正确自适应一般是superview.bounds未改变，设置subview frame的时机不对。


*如果想让某一个ViewController固定某个方向不旋转，方法如下：
1.修改AppDelegate.m，加入下列代码，其中_enablePortrait为新增的变量，用于判断是否要进行旋转。
```
- (NSUInteger)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
{
    if(_enablePortrait)
    {
        return UIInterfaceOrientationMaskPortrait;
    }
        
    return UIInterfaceOrientationMaskLandscape | UIInterfaceOrientationMaskPortrait;

}
```
2.在不需要旋转的viewController中的下列方法中加入以下代码即可。
```
-(void)viewWillAppear:(BOOL)animated
{
    AppDelegate *delegate = (AppDelegate *)[UIApplicationsharedApplication].delegate;
    delegate.enablePortrait = YES;

}



- (void)viewWillDisappear:(BOOL)animated
{
    [super viewWillDisappear:animated];
    AppDelegate *delegate = (AppDelegate *)[UIApplicationsharedApplication].delegate;
    delegate.enablePortrait = NO;
}
```

由于使用pushViewController会导致所进入的视图会根据前一视图的方向显示，所以需要用以下方法hack一下，才能使其自动根据设定的方向旋转。
```
- (void)updateOrientation
{
    [[UIApplicationsharedApplication] setStatusBarOrientation:UIInterfaceOrientationPortraitanimated:NO];
    UIViewController *viewController = [[UIViewControlleralloc] init];
    [self presentModalViewController:viewController animated:NO];
    [self dismissModalViewControllerAnimated:NO];
    [viewController release];
}
```


*iOS6旋转发生当时屏幕不旋转的原因可能是：
```
        if (SYSTEM_VERSION_GREATER_THAN_OR_EQUAL_TO(@"6.0")) {
            self.window.rootViewController = gameNavController;
        }else {
            [self.window addSubview:gameNavController.view];

        }
```

在应用中有时需要制定某些页面是Portrait或者landscape，这时需要在info.plist文件加入对这些方向的支持。
如果window的rootViewController是NavigationController则需继承该类写入：
```
//iOS6以下版本
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation
{
    return UIInterfaceOrientationIsLandscape(toInterfaceOrientation);
}
//iOS6及以上版本
- (BOOL)shouldAutorotate
{
    return YES;
}

- (NSUInteger)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskLandscape;

}
```


由此则全局默认情况下只支持landscape。
注意：navigationController在其子类中指定，在push进去的viewController指定则是无效。


有效的情况为使用presentModalViewController或者其他形式的present，在present的viewController中重写这三个方法，可以限制其当前的方向只为portrait.
```
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation
{
    return UIInterfaceOrientationPortrait == toInterfaceOrientation;
}

- (BOOL)shouldAutorotate
{
    return YES;
}

- (NSUInteger)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskPortrait;

}
```
