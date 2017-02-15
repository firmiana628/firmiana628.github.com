---
layout: post
title: virtualenv python虚拟环境
subtitle: 如何使用python虚拟环境
author: 张大威
date: 2016-09-23 15:21:00 +0800
categories: jekyll update
tag: python
---

<h3>安装虚拟环境</h3>
python虚拟环境是Python解析器的一个一个私有副本.在这个环境中安装私有包,不会影响系统中全局的python解释器.

虚拟环境非常有用,可以在系统的 Python 解释器中避免包的混乱和版本的冲突。为每个程 序单独创建虚拟环境可以保证程序只能访问虚拟环境中的包,从而保持全局解释器的干净 整洁,使其只作为创建(更多)虚拟环境的源。使用虚拟环境还有个好处,那就是不需要 管理员权限。

虚拟环境使用第三方实用工具 virtualenv 创建。输入以下命令可以检查系统是否安装了 virtualenv:

     <code>$ virtualenv --version</code>
     
如果结果显示错误,你就需要安装这个工具。
Python 3.3通过venv模块原生支持虚拟环境,命令为pyvenv。pyvenv可以替 代virtualenv。不过要注意,在Python 3.3中使用pyvenv命令创建的虚拟环 境不包含 pip,你需要进行手动安装。Python 3.4 改进了这一缺陷,pyvenv 完 全可以代替 virtualenv。

Linux 发行版都提供了 virtualenv 包。例如,Ubuntu 用户可以使用下述命令安 装它:
```
$ sudo apt-get install python-virtualenv
```
Mac OS X 系统,可以使用 easy_install 安装 virtualenv:
```
	$ sudo easy_install virtualenv
```


安装virualenv后,cd到工程目录,使用

```
 $ virtualenv venv
```

为工程配置虚拟环境.



Linux 和macos 使用      
```
$ source venv/bin/activate 

```

激活虚拟环境,激活后,命令行提示符会变为  : ` (venv) $ :`
退出虚拟环境:$: deactivate
<h3>安装pip</h3>
如果需要安装pip,需要去[pip网站](https://pip.pypa.io/en/latest/installing.htm)手动安装。 在 Python 3.4 中,pyvenv 会自动安装 pip。
<h3>使用pip</h3>
<code>(venv) $ pip install flask</code>

<h3>更改pycharm引用地址解释器地址和使用pycharm安装第三方包</h3>

![](/img/post/pycharm_interpreter.png)
如上图,更改 Project interpreter 为虚拟环境地址.如果不更改此处地址,那么在虚拟环境中安装的包,无法import.
左下角+号,可以安装第三方包


生成配置文件 (venv) $ pip freeze >requirements.txt
使用配置文件   (venv) $ pip install -r requirements.txt




