---
layout: post
title: "在Linux系统下配置avr开发环境"
category: other
tags: [AVR,LIUNX,gcc,avr-gcc]
modified:
image:
  feature: article.jpg
comments: true
share: true
---

###配置avr-gcc以及相关工具  
1.`$sudo apt-get install gcc-avr` 安装avr-gcc  
2.`$avr-gcc -v` 查看版本，如果版本低于4.6，请看[这里]  
3.`$sudo apt-get install avr-libc` 安装avr-libc  
4.`sudo apt-get install avrdude` 安装avrdude  

有一些工程使用了`scons` 那么直接使用`sudo apt-get install scons`即可  


###下载不到最新avr-gcc解决方案   
这个可能和您使用的ubuntu(或其他linux)源有关系，可以尝试添加新的源。  
1.`sudo cp /etc/apt/sources.list /etc/apt/sources.list.old` 备份原来的源  
2.`sudo gedit /etc/apt/sources.list` 使用gedit编辑器修改源列表(可以替换gedit为任何编辑器)  
3.推荐的源  

**网易的源**  

~~~~~~~~~~~~~~
deb http://mirrors.163.com/ubuntu/ raring main universe restricted multiverse  
deb-src http://mirrors.163.com/ubuntu/ raring main universe restricted multiverse  
deb http://mirrors.163.com/ubuntu/ raring-security universe main multiverse restricted  
deb-src http://mirrors.163.com/ubuntu/ raring-security universe main multiverse restricted  
deb http://mirrors.163.com/ubuntu/ raring-updates universe main multiverse restricted  
deb http://mirrors.163.com/ubuntu/ raring-proposed universe main multiverse restricted  
deb-src http://mirrors.163.com/ubuntu/ raring-proposed universe main multiverse restricted  
deb http://mirrors.163.com/ubuntu/ raring-backports universe main multiverse restricted  
deb-src http://mirrors.163.com/ubuntu/ raring-backports universe main multiverse restricted  
deb-src http://mirrors.163.com/ubuntu/ raring-updates universe main multiverse restricted  
~~~~~~~~~~~~~~~~~~~~

**上海交通大学（教育网速度极快)**  

~~~~~~~~~~~~~~~
deb http://ftp.sjtu.edu.cn/ubuntu/ raring main multiverse restricted universe  
deb http://ftp.sjtu.edu.cn/ubuntu/ raring-backports main multiverse restricted universe  
deb http://ftp.sjtu.edu.cn/ubuntu/ raring-proposed main multiverse restricted universe  
deb http://ftp.sjtu.edu.cn/ubuntu/ raring-security main multiverse restricted universe  
deb http://ftp.sjtu.edu.cn/ubuntu/ raring-updates main multiverse restricted universe  
deb http://ftp.sjtu.edu.cn/ubuntu-cn/ raring main multiverse restricted universe  
deb-src http://ftp.sjtu.edu.cn/ubuntu/ raring main multiverse restricted universe  
deb-src http://ftp.sjtu.edu.cn/ubuntu/ raring-backports main multiverse restricted universe  
deb-src http://ftp.sjtu.edu.cn/ubuntu/ raring-proposed main multiverse restricted universe  
deb-src http://ftp.sjtu.edu.cn/ubuntu/ raring-security main multiverse restricted universe  
deb-src http://ftp.sjtu.edu.cn/ubuntu/ raring-updates main multiverse restricted universe  
~~~~~~~~~~~~~~~~~~~~
回到[开始]重新下载安装，系统会删除avr-libc，所以你需要再安装一下，其他的不需要重新安装。  


###尝试从源码更新gcc遇到的问题  
从搜索结果来看，种种迹象表面avr-gcc和gcc有些关系，目前来讲我比较糊涂，反正直到最后成功更新了gcc至最新版本后，avr-gcc的版本仍然是老样子。不过从源码更新gcc还是能学到不少东西的。  

1.从`http://gcc.gnu.org/`下载gcc    

2.如果直接编译安装gcc，会提示` gcc configure: error: Building GCC requires GMP 4.2+, MPFR 2.3.1+ and MPC 0.8.0+`,此时有两种可能，一个是你没有安装新版本GMP,MPFR,MPC，一个是你安装了但是gcc编译的时候没有找到他们。  
如果没有安装，在一下三个站点可以分别下载到。如果已经安装，请看[这里](#1)  
`http://gmplib.org/`，`http://www.mpfr.org/`，`http://www.multiprecision.org/`

3.另外还需要安装M4文件以防止安装GMP出错    
`http://ftp.gnu.org/gnu/m4/`下载并安装     
`tar zxvf m4-x.x.x.tar.gz` x为对应版本号  
