---
layout: post
title: IOS常用申明
category: ios
---

#### 1. extern： 全局变量

  什么是全局变量：也称之为外部变量，是在方法外部定义的变量。它不属于哪个方法，而是属于整个源程序。
作用域是整个源程序。

  如果全局便利和局部变量重名，则在局部变量作用域内，全局变量被屏蔽，不起作用。编程时候尽量不使用全
局变量。

  全局变量通常放到接口文件中，在定义时不能对其进行初始化，否则就出错；
  
  	extern NSString *url;
  	
  	
#### 2. static：静态变量

什么是静态变量：从面向对象的角度触发，当需要一个数据对象为整类而非某个对象服务，同时有力求不破坏类的封装性，既要求此成员隐藏在类的内部，有要求对外不可见的时候，就可以使用static。

静态变量的优点：
	
  1. 节省内存。静态变量只存储一处，但供所有对象
  2. 它的值是可以更新的。
  3. 可提高时间效率。只要某个对象对静态变量更新一次，所有的对象都能访问更新后的值。 


#### 3. define： 定义

define与typedef功能类似，但它除了定义数据类型外，还可以定义给变量、语句等等定义，还可以包含参数。#define的原理是文本替换

#### 4. typedef：

用来声明新的类型名来代替已有的类姓名，例如：typedef int   CHANGE;指定了用CHANGE代表int类型，CHANGE代表int，那么：int a,b;和CHANGE a,b；是等价的、一样的。

#### 5. enum：枚举类型

typedef enum定义了枚举类型

	enum AlertTableSections
	{
		kUIAction_Simple_Section = 0,
		kUIAction_OKCancel_Section
	}; 

	typedef enum {
    	UIButtonTypeCustom = 0,           
    	UIButtonTypeRoundedRect,     
    	UIButtonTypeDetailDisclosure
	} UIButtonType;
	
#### 6. const 常量定义

 推荐定义方法：
 	
 	方法1. 接口文件中：extern const double EARTH_RADIUS;
 	      实现文件中：const double EARTH_RADIUS = 6353;
 	      
 	方法2. 接口文件中：static const int MY_CONSTANT_VARIABLE = 5 ; 