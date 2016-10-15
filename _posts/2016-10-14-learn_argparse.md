---
layout: post
title: argParse
subtitle: 命令行解析库 argparse
author: 张大威
date: 2016-10-14 23:48:08 +0800
categories: jekyll update
tag: python
---

<h2>前言</h2>
<p>argParses是一个脚本解析工具,可以实现允"像使用shell命令一样使用我们的脚本".比如<code>pod -v</code>是输出pod的version.<code>-v</code>就是一个arguement.如果我们有一个脚本文件"argPython.py",那么我们可以通过argParse 使 <code>$ argPython.py -v</code>成为可能.</p>

<h2>开始学习</h2>
<h4>安装argParse:</h4>
	
<code>$:pip install argParse</code>

<h4>使用</h4>
	1.import argparse // 导入模块
    2.parse=argparse.ArgumentParser() // 创建一个解析对象
   	3.parse.add_argument("echo",help="out put the arguemnt")  // 添加一个要解析的参数和参数的选项.每一个add_argument()对应一个一个参数    
	4.args=parse.parse_args() //解析

	5.arg.echo // 然后就可以使用参数了
    
   
   
 <h2>例子</h2>
 <h4>添加参数</h4>
  
``` 
import argparse
 
parse=argparse.ArgumentParser()

parse.add_argument("echo",help="out put the arguemnt")

args=parse.parse_args()
```


 输出:

```
	192:~ zhangdawei$ python /Users/zhangdawei/Desktop/python/imageToAsic/imageToAscII.py -h
usage: imageToAscII.py [-h] echo

positional arguments:
  echo        out put the arguemnt

optional arguments:
  -h, --help  show this help message and exit
192:~ zhangdawei$ 

```
 echo 是一个参数,help是对echo这个参数的说明.
 
 <h4>使用参数</h4>
 
 换一个参数:square
 
```
import argparse

parse=argparse.ArgumentParser()
parse.add_argument("square",help="out put square of given number",type=int)
args=parse.parse_args()
print args.square**2
```

 输出:

```
 
192:~ zhangdawei$ python /Users/zhangdawei/Desktop/python/imageToAsic/imageToAscII.py 2
4
```
这里添加了一个参数 `square ` 并且指定了它的类型 `type=int` .执行脚本时,添加参数2,输出了2的平方4.


如果参数非法,还会报错:

```
192:~ zhangdawei$ python /Users/zhangdawei/Desktop/python/imageToAsic/imageToAscII.py two
usage: imageToAscII.py [-h] square
imageToAscII.py: error: argument square: invalid int value: 'two'
```

<h4>可选参数</h4>
```
	import argparse

parse=argparse.ArgumentParser()
parse.add_argument("square", help="out put square of given number",type=int)
parse.add_argument("--verbose",help="let outPut verbosely", action="store_true")

args=parse.parse_args()
answer=args.square*2
if args.verbose:
    print 'the square of {} is:{}'.format(args.square,answer)
else:
    print answer

```


这里使用了可选参数"vserbose" ,并且指定 `action`为`store_true` ,意思就是说,当命令行中使用了verbose,就把arg.verbase指定为true


<h4>show option argment</h4>
如果并不想使用那么长的参数,还可以给参数指定一个短的名字.

```

parse.add_argument("-v","--verbose",help="let outPut verbosely", action="store_true")


```
这种情况下使用 `-v`和`--verbose`是等同的.



参考:

 <a src = "http://blog.ixxoo.me/argparse.html">http://blog.ixxoo.me/argparse.html</a>
 
 <a src="http://www.jianshu.com/p/a50aead61319">http://www.jianshu.com/p/a50aead61319</a>
 
 <a>http://www.2cto.com/kf/201412/363654.html</a>




