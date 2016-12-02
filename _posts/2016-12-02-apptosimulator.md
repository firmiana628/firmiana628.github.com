---
layout: post
title: 在模拟器上安装package
subtitle: 
author: 张大威
date: 2016-12-02 11:30:42 +0800
categories: jekyll update
tag: iOS
---
<p>公司测试机不够,设计师又没有苹果手机,问我们如何解决.不想把源码给设计师,就研究了一下如何在模拟器上安装应用</p>
<p>xcode run之后,在DrivedData(~/Library/Developer/Xcode/DerivedData 
)下会有一个app的包,把这个包copy到模拟器的应用路径下就可以了
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator.sdk/Applications 
</p>
<p>又试了一下微信的ipa包,并不能适用,在模拟器上打开立即闪退了</p>