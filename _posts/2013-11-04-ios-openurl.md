---
layout: post
title: Hello world!
category: IOS学习记
---

####1. 在plist文件中，注册对外接口

在xcode group&files 里面，展开 resources选择info.plist 鼠标右击information property list ，然后从列表中选择URL types 右击 add row 添加一个对象（item）右击item add row 从列表中选择 URL Schemes 再右击添加一个对象（item1） 将item1得值设置为：myapp 这个myapp就是对外接口，其它应用可以通过它，调用该应用

#### 2. 处理URL请求

应用程序委托在 application:handleOpenURL:方法中处理传递给应用程序的URL请求。如果您已经为自己 的应用程序注册了定制的URL模式，则务必在委托中实现这个方法。 下面代码实现了这个委托方法:

	-(BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url{
		if([[url scheme] isEqualToString:@"myapp"]){
			[application setApplicationIconBadgeNumber:10];
			return YES;}return NO;
		}

	调用：
		[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"程序的相应连接"]];


1）调用 自带mail: 
`[[UIApplicationsharedApplication] openURL:[NSURLURLWithString:@"mailto://admin@hzlzh.com"]]`;

2）调用 电话phone: 
`[[UIApplication sharedApplication] openURL:[NSURLURLWithString:@"tel://8008808888"]]`;

3）调用 SMS: 
`[[UIApplicationsharedApplication] openURL:[NSURL URLWithString:@"sms://800888"]]`;

4）调用自带浏览器 safari: 
`[[UIApplicationsharedApplication] openURL:[NSURLURLWithString:@"http://www.hzlzh.com"]]`;

5）调用 Remote: 
`[[UIApplicationsharedApplication] openURL:[NSURL URLWithString:@"remote://fff"]]`;

6）调用去 appstore: 
`[[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"itms-apps://itunes.apple.com/app/{appid} "]]`;