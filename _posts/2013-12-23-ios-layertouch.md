---
layout: post
title:  利用事件和layer来解决view超出父view不可点击问题
category: ios
---

大家都很清楚，假设一个UIView 为 A，另外一个 view 为 B，B 为 A 的 subView，当 B 的一部分视图超出 A 之后，超出部分就没有点击事件了，只有在 A view 里的部分可以响应点击事件，大概样子如下：

![image](https://f.cloud.github.com/assets/1698185/1799986/da09af58-6bb4-11e3-8940-7495224b1fbb.png)

下面直接上代码，大家一看便能够看懂：

	@interface CALayerTestViewController ()
	{
    	UIView *supView;
    	UIButton *subBtn;
	}
	@end

	@implementation CALayerTestViewController

	- (void)viewDidLoad
	{
    	[super viewDidLoad];
    	//定义一个父view
    	supView = [[UIView alloc] initWithFrame:CGRectMake(30, 50, 200, 100)];
    	supView.backgroundColor = [UIColor redColor];
    
    	//定义一个子 view，我这里直接用了个按钮
    	subBtn = [[UIButton alloc] initWithFrame:CGRectMake(-10, -10, 20, 20)];
    	subBtn.backgroundColor = [UIColor greenColor];
    	[subBtn setTitle:@"X" forState:UIControlStateNormal];
    
    	//测试如果利用添加 view 的方法，点击效果
    	//[subBtn addTarget:self action:@selector(close) forControlEvents:UIControlEventTouchUpInside];
    
    	//如果直接加为 view，超出 A 的部分将不会显示
    	//[supView addSubview:subBtn];
    
    	//利用 layer + 事件来解决
    	[supView.layer addSublayer:subBtn.layer];
    
    	[self.view addSubview:supView];
	}

	//响应 touch 事件，做 view 位置判断
	- (void)touchesBegan:(NSSet*)touches withEvent:(UIEvent*)event
	{
    	CGPoint p = [(UITouch*)[touches anyObject] locationInView:supView];
    	for (CALayer *layer in supView.layer.sublayers) {
        	if ([layer containsPoint:[supView.layer convertPoint:p toLayer:layer]]) {
            	[self close];
        	}
    	}
	}

	- (void)close
	{
    	NSLog(@"close");
	}

	@end

今天在看 layer 相关的东西时，临时记起同事之前遇到过一个类似的问题，不过他那个场景比较特殊所以解决方法可以用，但是不太灵活。如果大家有更好的方式，欢迎回复。