---
layout: post
title: 使用javaScriptCore完成iOS开发中JS和native的交互
subtitle: javaScript Objective-C bridge
author: 张大威
date: 2016-06-19 21:51:49 +0800
categories: jekyll update
tag: iOS
---

#使用javaScriptCore完成iOS开发中JS和native的交互

##开篇

  很多的app都会使用wap页面,一方面是app上线后台可配置,一方面是Android和iOS可重用.这时JS和原生代码之间数据的传递,就变得尤为重要.而JavaScriptCore.framework这个库,就在iOS平台中扮演着JS和native交互的桥梁的角色.
  
  之前的项目代码采用的方法是在webView的代理方法中拦截请求.
  
  `-(void)webViewDidFinishLoad:(UIWebView *)webView`
  
  通过URL的scheme判断是否是用作交互的URL,通过query传递数据.这种方式解决了JS和iOS交互的问题, 但是也有两个缺点:
  
  * 代码繁琐,不够优雅
  * JS经常会调不到native方法,和前端的同学沟通发现,JS的onclick方法要setTimeOut 0.5毫秒,native方法才能够被调用.

我们一直在寻求更好的方法,知道发现了JavaScriptCore.framework.

##JavaScriptCore






##代码实现
