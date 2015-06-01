---
layout: post
title: "在VS2010中使用QextSerialPort 开发QT串口程序"
category: other
tags: [C++,QT,串口，编译]
image:
  feature: article.jpg
comments: True
share: true
---

-----------------------
**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)

------------------


##编译QextSerialPort
参考链接：[How To: QextSerialPort for Visual Studio 2010](http://www.holoborodko.com/pavel/2011/02/01/how-to-compile-qextserialport-wit-visual-studio-2010/)

###1.克隆源码

```
git clone https://github.com/qextserialport/qextserialport.git
```

###2.进入目录后生成VS2010工程文件

```
qmake -tp vc qextserialport.pro
```
则可以生成VS2010所需的工程文件，如图所示，使用VS打开
![Alt text](/images/QextSerialPort/1433083199586.png)

当然，如果要生成64位的，则需要使用64位QT中提供的qamke，关于如何编译64位QT，请看[这里](http://hanxiaomax.github.io/other/build-qtx64-win7/)

###3.配置输出目录
默认的编译模式是Debug和Win32，如果你需要编译64位，请新建一个x64的编译模式。
如果你的编译模式，和你工程文件所指定的模式不同，则会提示**fatal error LNK1112: module machine type 'x64' conflicts with target machine type 'X86'**

- 右键工程，选择属性，把General 中的 Target Name:
从qextserialportd1改为 qextserialportd。
- Linker -> General -> Output file中**$(OutDir)\qextserialportd1.dll**改为**build\qextserialportd.dll**

同样设置release中这两个位置的值。需要注意的是，release中的**qextserialport**，结尾是没有`d`的。

###4.编译

右键工程文件，选择编译，会要求我们保持解决方案文件，点击保存即可。

分别编译debug和release版本，在build文件夹下生成如下文件：
![Alt text](/images/QextSerialPort/1433083922713.png)

- 把全部文件拷贝到你的QT安装目录下的Lib文件夹中
- 把dll文件拷贝到你的QT安装目录下的bin文件夹中

###5.在项目中使用

- 在项目中使用QextSerialPort库，一般我们会把QextSerialPort源码包中的src文件夹拷贝到项目目录下3rdparty文件夹下，表示其是一个第三方库。

- 包含头文件**#include "3rdparty/qextserialport/qextserialport.h"**
- 右键项目，属性，Linker里面的Input中，我们要添加库文件，否则会出现**未解析的外部变量**这个链接错误。
    - debug版本添加**qextserialportd.lib**
    - release版本添加**qextserialport.lib**


####VS2010预编译64位QextSerialPort可以在此直接下载：[链接](http://pan.baidu.com/s/1o6onhPK) 密码: wtsi
