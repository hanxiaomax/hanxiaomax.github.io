---
layout: post
title: "腾讯云配置：Flask+uWSGI+Nginx"
category: other
tags: [Python,Flask,Nginx]
image:
  feature: article.jpg
comments: True
share: true
---


**操作系统为Ubuntu**

##1.安装Python虚拟环境

首先你要有pip，没有的话搜索一下如何安装，pip是Python的一个包管理工具

1. 安装虚拟环境virtualenv
`sudo pip install virtualenv`
2. 新建虚拟环境  
- `cd /home/ubuntu/flaskCard/`
- `virtualenv env` 
3. 激活虚拟环境
`source env/bin/activate/`

##2.安装Flask相关包

这里根据自己需要安装，也可以用requirements.txt来简化安装过程

##3.安装uWSGI

`pip install uwsgi`

##4.uWSGI配置文件  
在项目根目录新建**uwsgi_config.xml**  

```
<uwsgi><pythonpath>/home/ubuntu/flaskCard/</pythonpath> #网站根目录
     <module>run</module>     #Flask的主入口文件，平时是直接运行这个文件启动测试服务器的
     <callable>app</callable>   #runServer.py入口文件里的程序入口
     <socket>127.0.0.1:5000</socket>       #监听端口
     <master/>
     <processes>4</processes>                #注：跑几个线程，这里用4个线程
     <memory-report/>
</uwsgi>
```

我们暂时还不去启动uWSGI，启动uWSGI的时候，网站就可以访问了，使用命令为: 

`uwsgi -x uwsgi_config.xml `

##5.安装Nginx

`sudo apt-get install Nginx`

##6.配置Nginx

Nginx的配置文件在**/home/ubuntu/etc/nginx/sites-available**下，这里我们直接替换其中的default

Nginx站点配置：


```
server{
                listen       80;
                server_name xxx.xxx.xxx;
                error_log /home/ubuntu/flaskCard/logs/error.log;
                access_log  /home/ubuntu/flaskCard/logs/access.log;
                root  /home/ubuntu/flaskCard/;
                location / {
                        include uwsgi_params;
                        uwsgi_pass 127.0.0.1:5000;
                        #uwsgi_pass unix:/tmp/uwsgi.sock;
                }
        }
```

配置完成后重新启动服务：

`sudo service nginx restart`

注意点：

- server_name 填写网址，当然可以是ip地址，但是填写ip地址后，启动服务会报错：**could not build the server_names_hash, you should increase server_names_hash_bucket_size: 32**，此时我们需要打开nginx.conf，并在其中http下添加**server_names_hash_bucket_size 64;** 
- uwsgi_pass 可以是ip也可以说.sock但是后者可能会出现权限问题，需要赋权限


##7.安装Supervisor

`sudo apt-get install supervisor`

##8.配置Supervisor

我们需要在**/etc/supervisor/conf.d/**下建立一个新的配置文件：**flaskcard_supervisor.conf**

`cd /etc/supervisor/conf.d/`

```
[program:flaskcard]
# 设置启动uWSGI所需的命令
command=/home/ubuntu/flaskCard/env/bin/uwsgi -x /home/ubuntu/flaskCard/uwsgi_config.xml

# 命令程序所在目录
directory=/home/ubuntu/flaskCard
#运行命令的用户名
user=root
    
autostart=true
autorestart=true
#日志地址
stdout_logfile=//home/ubuntu/flaskCard/logs/flaskcard_supervisor.log
```

##9.启动网站

调试到时候可以单独启动，使用：

`uwsgi -x uwsgi_config.xml `

上线的时候使用Supervisor来代替我们启动uwsgi，当应用崩溃的时候可以自动重启，保证可用性

启动：`sudo service supervisor start`
停止：`sudo service supervisor stop`


--------------------
##杂项

- `killall -9 uwsgi`可以停止uwsgi进程
- `lsof -i tcp  / grep LISTEN`  可以查看端口监听情况
- `find /path/to/find/with/ name xxx`
