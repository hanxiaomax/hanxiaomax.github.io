---
layout: post
title: "#Cura二次开发环境配置"
category: other
tags: [Cura,3D打印]
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

##安装Git for Windows
[下载地址](https://msysgit.github.io/)
安装完成之后，鼠标右键可以打开git bash，用于输入下面的命令。

##获取Cura源代码

###fork and clone

首先我们需要`fork` [Cura](https://github.com/daid/Cura)，并将fork后的repo `clone`到本地

```
git clone https://github.com/hanxiaomax/Cura.git
```

##配置开发环境
**以64位 Windows 7为例**

首先需要安装python，强烈推荐安装32位的，不论你是64位系统还是32位系统，都可以使用32位Python。
###0.创建虚拟环境
首先利用pip安装virtualenv，网上很多教程这里不细说了，然后进入cura目录创建虚拟环境 **mycura**
```
virtualenv mycura
```

创建虚拟环境可以安装的依赖库到虚拟环境中，比较容易清理而且也不会把系统的python库搞得乱七八糟。

```
source mycura/Scripts/activate
```
激活虚拟环境，Mac OS 和 Linux这里命令稍有不同。

创建并激活虚拟环境后，就要安装我们所需要的所有依赖包。

###1.安装wxpython

Cura的GUI是用python框架wxPython开发的，而wxPython不可以从pip安装，只能从源码编译或者安装预[编译版本](http://www.wxpython.org/download.php#msw)

wxPython不能直接从pip安装，在虚拟环境中安装看cura教程，比较复杂，需要从源码编译。比较简单的办法就是先把wxPython安装到系统（我们采用这样的方法），然后从**C:\Python27\Lib\site-packages**中把**wx-3.0-msw**，**wx.pth**,**wxversion.py**这三个拷贝到虚拟环境下面的Lib/site-packages中即可在虚拟环境中使用。


如果出现以下问题：**DLL load failed: 1% 不是有效的win32 应用程序
![Alt text](./1432018832187.png)

说明wxPython和Python的版本不匹配，此处应该安装32位的wxPython

安装完成后，进入Python，运行一下import wx，如果没有出错就是成功了。（注意测试时应该处于虚拟环境中）

###2.安装其他依赖库
在cura的目录内，使用如下命令，安装**requirements.txt**文件指明的全部依赖包。
`pip install -r requirements.txt`

如果是在windows平台下，很有可能会无法安装numpy。我们这里选择手动安装到系统中，然后拷贝到虚拟环境下的库中。
在windows平台上编译容易出现问题，且需要配置很多工具。因此我们直接从网上下载编译好的numpy。

**注意，预编译的numpy只有win32版本，需要32位的python，因此我建议使用32位的python。否则还是要自己手动编译的。**

安装完成后从系统的**C:\Python27\Lib\site-packages**中把**numpy**和**numpy-1.9.2-py2.7.egg-info**拷贝到虚拟环境下的Lib/site-packages即可。

同样的，进入Python然后`import numpy`测试是否安装成功，成功后，从requirements.txt中删除numpy，再次运行，安装其他的依赖库。

##运行Cura
###0.修改app.py

我们并不希望，每次修改代码后，都要对Cura重新打包然后运行查看效果，而是希望有更加方便的预览办法。其实非常简单，只需要把Cura当做一个模块运行即可。


~\Cura\gui\app.py 中我们需要添加几行代码，使其可以作为模块单独运行

```
if __name__ == '__main__':
    app=CuraApp("1.txt")#随便指定一个文件即可
    app.MainLoop()#开启wx的主循环
```

###1.启动

在cura根目录下
```
python -m Cura.gui.app
```
会出现splash画面，如果没有进一步启动程序，说明有一些脚本无法载入，一般是因为导入不了某些库。此时我们需要检查一下是否所有的依赖都已经安装。。正常情况应该是在稍许延时后完成启动。


---------------------------------------------

我会在接下来的一篇文章里面，详细介绍一下二次开发Cura的思路，包括软件的整体架构，分析各个文件的用途。
