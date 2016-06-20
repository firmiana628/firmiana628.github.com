---
layout: post
title: 使用javaScriptCore完成iOS开发中JS和native的交互
subtitle: javaScript Objective-C bridge
author: 张大威
date: 2016-06-19 21:51:49 +0800
categories: jekyll update
tag: iOS
---

<h1>使用javaScriptCore完成iOS开发中JS和native的交互</h1>

<h2>开篇</h2>

  很多的app都会使用wap页面,一方面是app上线后台可配置,一方面是Android和iOS可重用.这时JS和原生代码之间数据的传递,就变得尤为重要.而JavaScriptCore.framework这个库,就在iOS平台中扮演着JS和native交互的桥梁的角色.
  
  之前的项目代码采用的方法是在webView的代理方法中拦截请求.
  
  `-(void)webViewDidFinishLoad:(UIWebView *)webView`
  
  通过URL的scheme判断是否是用作交互的URL,通过query传递数据.这种方式解决了JS和iOS交互的问题, 但是也有两个缺点:
  
  * 代码繁琐,不够优雅
  * JS经常会调不到native方法,和前端的同学沟通发现,JS的onclick方法要setTimeOut 0.5毫秒,native方法才能够被调用.

我们一直在寻求更好的方法,直到发现了JavaScriptCore.framework.

<h2>JavaScriptCore</h2>

javaScriptCore 是用来支持js和native交互的库.通过JavaScriptCore Framework,可以用OC执行JS,或者在JS中注入OC对象.

*图一*

![JavaScriptCore.h](/img/post/jsCore_interface.png)

在JavaScriptCore头文件中可以看到,JavaScriptCore主要由以下几个类构成

* JSContext  
　　　JSContent 对象提供一个JS运行环境.
* JSValue    
　　　JSValue对象是一个OC和JS数据类型相互转换的桥梁.比如string->NSString,NSString->string.对应关系及转化方法如下表:
　　　
　　　![map between oc and js](/img/post/jsCore_mapValues.png)
　　　
* JSManagedValue
* JSVirtualMachine
* JSExport JSExport 实现了一个protocol.下面会主要用到这个协议.


<h4>JSExport</h4>
JSExport提供了一个把OC的对象的类方法实例方法\属性注入到JS的方式.当一个实例遵循某个协议,而这个协议又继承自**JSExport**的时候,就可以把协议中的方法注入到JS中,JSCore会自动的把OC的数据类型转化为JS数据类型.这么说会有点绕,看下面的代码:

	#import <Foundation/Foundation.h>
	#import <JavaScriptCore/JavaScriptCore.h>

	@protocol JsNativeBridgeDelegate <JSExport>
	-(void)testLog:(NSString *)string;`
	-(void)testTwo:(NSString *)str ArgLog:(NSInteger)count;
	@end

	@interface JsNativeBridge : NSObject<JsNativeBridgeDelegate>
	+(instancetype)shareInstance;
	@end

 JsNativeBridgeDelegate继承自协议JSExport,而JsNativeBridge有遵循了协议JsNativeBridgeDelegate.这时,把JsNativeBridge的对象注入JS后,就可以通过instnce.function的形式调用JsNativeBridgeDelegate协议里的方法.
 
 
在webView加载完成的代理中,取到wap的上下文,并向其中注入JsNativeBridge对象.

``` 
#pragma mark -- webView delegate
-(void)webViewDidFinishLoad:(UIWebView *)webView
{
     JSContext *jscontext = [webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    
    JSValue *jsObject = jscontext[@"iosJsBridge"];
    if (![jsObject toBool]) {
        jscontext[@"iosJsBridge"] = [JsNativeBridge shareInstance];
    }
}
```
在html页面,就能够用iosJsBridge对象去调用JsNativeBridgeDelegate的方法了.

`<button onclick="iosJsBridge.testLog('调用native方法')">jsToNative -testLog</button> `

这样就能够调用到-(void)testLog:(NSString *)string;方法.参数为@"调用native方法".

当方法有多个参数时,JSCore会把OC的方法名中的":"(冒号)去掉,并且冒号后面第一个字母大写,转化为JS的方法.如果要调用`-(void)testTwo:(NSString *)str ArgLog:(NSInteger)count;`,在JS中就要这么写:`iosJsBridge.testTwoArgLog('调用native多参数方法',100)`.

当方法中有多个参数的时候,OC的方法名就会变得很长,为了简便,JSCore提供了一个宏**
JSExportAs(PropertyName, Selector) **,可以简化方法名. JS通过`iosJsBridge.PropertyName`调用方法Selector.

JSCore也可以把函数直接注入到JS中,或者很方便的执行JS方法.有时间再写吧....

