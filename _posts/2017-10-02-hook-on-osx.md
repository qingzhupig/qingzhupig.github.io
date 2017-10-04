---
layout: post
title: "OSX的Hook"
categories: osx, hook
---

## Hook原理

在启动一个可执行文件之前，通过设置`DYLD_INSERT_LIBRARIES`环境变量来加载一个动态库，并且在动态库中利用`Objective-C`的`Runtime`特性来实现函数的`Hook`。


`Hook`工具 swizzle.h

``` c
#import <objc/runtime.h>

IMP SwizzleClassSelector(Class aClass, SEL selector, IMP newImplementation)
{
  Class metaClass = objc_getMetaClass(class_getName(aClass));
  Method method = class_getClassMethod(metaClass, selector);
  IMP origImp = method_getImplementation(method);
  if (! origImp) {
    NSLog(@"failed to replace class selector: %p", selector);
    return NULL;
  }

  class_replaceMethod(metaClass, selector, newImplementation, method_getTypeEncoding(method));
  return origImp;
}

IMP SwizzleSelector(Class aClass, SEL selector, IMP newImplementation)
{
  Method method = class_getInstanceMethod(aClass, selector);
  IMP origImp = method_getImplementation(method);
  if (! origImp) {
    NSLog(@"failed to replace selector: %p", selector);
    return NULL;
  }

  class_replaceMethod(aClass, selector, newImplementation, method_getTypeEncoding(method));
  return origImp;
}
```

获取`Class`对象和`SEL`

当可以访问到类和方法定义时，直接用`[XXX class]`来获取类对象, 用`@selector(xxx)`获取`SEL`；但是通常在进行`Hook`的时候，是没有具体的类定义的，这个时候可以用`objc_getClass("xxx")`获取类对象，用`NSSelectorFromString(@"xxx")`获取`SEL`。

`__attribute__`

一句话来说，`__attribute__((constructor))`在动态库加载之后`main`方法执行之前执行，`__attribute__((destructor))`在程序退出之前执行。

参考

* [JRSwizzle](https://github.com/rentzsch/jrswizzle)
* [Initializing a Framework at Runtime](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPFrameworks/Tasks/InitializingFrameworks.html)
* [理解Objective-C Runtime](http://www.cocoachina.com/ios/20141008/9844.html)
* [Objective-C深入理解+load和+initialize](http://www.jianshu.com/p/872447c6dc3f)
* [Objective-C的hook方案: Method Swizzling](http://blog.csdn.net/yiyaaixuexi/article/details/9374411)

## Hook示例

demo.h

``` c
#import <Foundation/Foundation.h>

@interface Demo : NSObject
- (NSString *)run:(NSString *)message;
@end
```

demo.m

``` c
#import "demo.h"

@implementation Demo
- (NSString *)run:(NSString *)message
{
  NSLog(@"hello, %@", message);
  return message;
}
@end
```

main.m

``` c
#import <Foundation/Foundation.h>
#import "demo.h"

int main(int argc, char **argv)
{
  NSLog(@"executing main");

  Demo *demo = [[Demo alloc] init];
  [demo run:@"demo"];
  [demo release];

  return 0;
}
```

hook.m

``` c
#import <Foundation/Foundation.h>
#import "swizzle.h"

static NSString *(*original_demo_run_impl)(id self, SEL _cmd, NSString *prefix) = NULL;
static NSString *swizzled_demo_run_impl(id self, SEL _cmd, NSString *prefix)
{
  NSLog(@"executing hooked demo run:");
  return original_demo_run_impl(self, _cmd, prefix);
}

__attribute__((constructor)) void init()
{
  NSLog(@"executing constructor");
  original_demo_run_impl = (NSString *(*)(id, SEL, NSString *))
    SwizzleSelector(objc_getClass("Demo"), NSSelectorFromString(@"run:"), (IMP)swizzled_demo_run_impl);
}

__attribute__((destructor)) void destory()
{
  NSLog(@"executing destructor");
  original_demo_run_impl = NULL;
}
```

编译并执行

``` bash
$ gcc -framework foundation -lobjc -o main main.m demo.m
$ ./main
2017-10-02 21:03:02.142 main[81239:681596] executing main
2017-10-02 21:03:02.143 main[81239:681596] hello, demo
$ gcc -framework founation -lobjc -dynamiclib -o hook.dylib hook.m
$ DYLD_INSERT_LIBRARIES=hook.dylib ./main
2017-10-02 21:05:12.516 main[81330:682462] executing constructor
2017-10-02 21:05:12.517 main[81330:682462] executing main
2017-10-02 21:05:12.517 main[81330:682462] executing hooked demo run:
2017-10-02 21:05:12.517 main[81330:682462] hello, demo
2017-10-02 21:05:12.517 main[81330:682462] executing destructor
```
