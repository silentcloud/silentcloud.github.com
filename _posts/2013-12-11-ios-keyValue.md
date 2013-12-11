---
layout: post
title: objective-c中的cocoa特性：KVC-键值编码（转）
category: ios
---


在oc中，可以使用KVC来访问变量的属性，即使该属性没有get,set方法也可以调用，方便灵活，另外还可以方便的管理集合，具体使用我们先看代码：
[plain] view plaincopy
	
	#import <Foundation/Foundation.h>  
	@interface Human:NSObject  
	{  
    	NSString *name;  
    	int age;  
    	Human *child;  
	}  
	//注意下面并没有写age的property  
	@property (copy) NSString *name;  
	@property (retain)Human *child;  
	
	@end  

	@implementation Human  
  
	@synthesize name;  
	@synthesize child;  
	
	@end  
  
	int main(int argc, const char * argv[])  
	{  
  
      @autoreleasepool {  
          
        Human *human = [[Human alloc]init];  
        Human *child=[[Human alloc]init];  
          
        [human setValue:@"holydancer" forKey:@"name"];//将name属性设置为"holydancer"  
        NSString *nameOfHuman=[human valueForKey:@"name"];//将human中的name属性取出  
        NSLog(@"%@",nameOfHuman);  
          
        [human setValue:[NSNumber numberWithInt:20] forKey:@"age"];//将没有set方法的age属性赋值  
        NSLog(@"%@",[human valueForKey:@"age"]);//将没有get方法的age值取出。  
          
        [human setValue:child forKey:@"child"];//等价于human.child=child;  
          
        //将human中的属性child的name属性设置为"dancer's child"          
        [human setValue:@"dancer's child" forKeyPath:@"child.name"];  
        //这里的方法名叫setValue:forKeyPath,可以用来设置当前对象属性的属性。  
          
        //将human中的属性child的age属性取出。          
       NSLog(@"%@",[human valueForKeyPath:@"child.name"]);  
             
      }  
      return 0;  
	}  
2012-03-20 19:58:30.634 kvc[3197:403] holydancer

2012-03-20 19:58:30.637 kvc[3197:403] 20

2012-03-20 19:58:30.638 kvc[3197:403] dancer's child

如上所示，例用键值编码可以很轻松地操作对象的属性和对象属性的属性。需要注意的是，因为KVC是cocoa的特性，所以在键值设置或者获取时，键统一是字符串，而值是不支持基本数据类型的，所以如上所示，我们需要将age包装成NSNumber类型，另外在输出age时，即使我们知道是int型，但取出时是按NSNumber操作的，输出占位符仍是%@.
下面我们来看看KVC的另一种简单用法，添加运算符：

	#import <Foundation/Foundation.h>  
	@interface Human:NSObject  
	{  
   		NSString *name;  
    	int age;  
    	NSMutableArray *children;  
	}
	
	//注意下面并没有写age的property  
	@property (copy) NSString *name;  
	@property (retain)Human *child;  
	@end  
		
	@implementation Human  
  
	@synthesize name;  
	@synthesize child;  
	@end  
  
	int main(int argc, const char * argv[])  
	{  
      @autoreleasepool {  
          
        Human *human = [[Human alloc]init];  
        Human *child1=[[Human alloc]init];  
        Human *child2=[[Human alloc]init];  
        [child1 setValue:[NSNumber numberWithInt:5] forKey:@"age"];  
        [child2 setValue:[NSNumber numberWithInt:10] forKey:@"age"];//给两个child的age属性赋值，因为没有property，所以用KVC模式赋值  
        NSMutableArray *children =[[NSMutableArray alloc]init];  
        [children addObject:child1];  
        [children addObject:child2];  
        //上面是为了将两个Human类包装为NSArray，准备放入human类  
        [human setValue:children forKey:@"children"];  
        NSNumber *count=[human valueForKeyPath:@"children.@count"];//利用键路径计算human对象中children属性包含的元素个数  
        NSNumber *sumOfAge=[human valueForKeyPath:@"children.@sum.age"];//计算children中所有对象的年龄和  
        NSNumber *maxOfAge=[human valueForKeyPath:@"children.@max.age"];//计算children中所有对象年龄最大的  
        NSNumber *minOfAge=[human valueForKeyPath:@"children.@min.age"];//计算children中所有对象年龄最小的  
        NSNumber *avgOfAge=[human valueForKeyPath:@"children.@avg.age"];//计算children中所有对象年龄的平均数  
        //@是运算符的  
        NSLog(@"children中有%@个元素，年龄和为%@，最大年龄为%@,最小年龄为%@,平均年龄为%@",count,sumOfAge,maxOfAge,minOfAge,avgOfAge);  
       
      }  
      return 0;  
    }  

2012-03-20 19:48:23.574 kvc[2996:403] children中有2个元素，年龄和为15，最大年龄为10,最小年龄为5,平均年龄为7.5