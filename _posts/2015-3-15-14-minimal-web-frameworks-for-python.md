---
layout: post
title: "14个Python微型框架"
category: other
tags: [Python,Web]
image:
  feature: article.jpg
comments: True
share: true

---

本文由 [伯乐在线 - 艾凌风](http://www.jobbole.com/members/hanxiaomax) 翻译，[进林](http://www.jobbole.com/members/8zjl8) 校稿。未经许可，禁止转载！
英文出处：[codecondo.com](http://codecondo.com/14-minimal-web-frameworks-for-python/)。欢迎加入翻译组。

---------------------------------


我最近发表了一篇名为 [7 Minimal Node.js Web Frameworks for 2014 and Beyond](http://codecondo.com/7-minimal-node-js-web-frameworks/)的博文——目前它是我博客访问量最高的文章：超过10000人浏览，分享和评论了这些我总结到一起的web框架。

这教会了我一些事，这类文章是有需求的——因为它提供了触手可及的备查和/或探索了做事情的新方式。我发现很多“周刊”在他们的新闻或是博客上刊登了我的文章，对此我感到很高兴。这促使我尝试把更多其他语言中的此类内容总结到一起，比如Python、Ruby、PHP和JavaScript。



众口难调，我尽量把我搜索的时候出现在靠前位置的那些框架都囊括进来了， 我试着以社区规模大小以及它们在GitHub上的活跃度来为它们排名。我建议您在留言中列出您个人最喜爱的框架（或是您正在使用的框架）的链接，我会尽快把它加入到列表中。


[Python](http://www.python.org/) 是一个可以让你更快地完成工作，更高效地整合系统的语言。你可以学习使用Python并且马上获得生产力的提升，降低维护成本。


Python版的Hello World程序



你可能忘记该怎么做了，下面是一个提示。 print "Hello World!";

我还特别喜欢这一段代码，
```python
  for i in ["/","*","|","","|"]:
        print "%sr" % i,
```


##Python的Web框架

当一些基础工作不需要你操心的时候，工作起来会比较容易，这也是为什么框架在各个语言的开发者社区中变得如此流行的原因，你无法否认的是，拼装一个网页或是一个项目，比起不得不创建你自己的类或方法要容易的多。


我秉承自己的承诺，在本文或是将来任何的榜单中，不偏向任何一个框架，所有的选择都基于我个人意见和喜好。如果你可以和朋友们分享本文，在你自己的博客上面宣传一下我会很感激你的。同时我也很感激那些让这些Python web框架成为可能所付出的辛勤劳动。

###[bobo](http://bobo.digicool.com/en/latest/index.html)

![Alt text](/images/python-frame/1.jpg)  

Bobo是一个轻量级的框架，用来创建WSGI web应用。它的目标是简单易用，容易记忆。

它强调两个方面的内容：

1)把URL映射到对象； 
2)调用对象来生成HTTP响应。

Bobo 并不具备模板语言，数据库集成层或是其他一些WSGI中间件或特定应用程序库所提供的功能。Bobo建立在其他框架之上，尤其是WSGI和WebOb。

###[Bottle](http://bottlepy.org/docs/dev/)
![Alt text](/images/python-frame/2.jpg)  
Bottle是一个快速、简单、轻量级的WSGI微型Python web框架。它仅包含单一文件模块，并且不依赖除了Python标准库以外的其他库。

它支持类似Google App Engine、Python Paste这样的应用，还包含了对一些模板的支持，比如Cheetah和Mako。

###[CherryPy](http://www.cherrypy.org/)
![Alt text](/images/python-frame/3.jpg)  
CherryPy 允许开发者以他们构建其他面向对象Python程序近乎同样的方式来开发web应用。这使得可以在更短的时间内开发出更精简的源代码。CherryPy允许你进行很多常规的Python编程，但是它并没有整合一个模板系统，你需要自己去找一个。（它支持大多数的模板）

CherrPy 能够很好适应默认的Python功能和结构，它在使用更少的代码创建web应用

###[Cyclone](http://cyclone.io/)
![Alt text](/images/python-frame/4.jpg)  
Cyclone 是一个Python的web服务框架，它基于Twisted protocol实现了Tornado API 。我将把对这个框架的介绍，交给7co.的Gleicon，请看他的文章。

###[Flask](http://flask.pocoo.org/)
![Alt text](/images/python-frame/5.jpg)  
Flask是一个基于Werkzeug 和 Jinja2的微型Python框架。它的目的是更快地上手，基于很多很好的想法开发出来的。你可以在 Wikipedia上了解更多内容。

###[Itty-Bitty](https://github.com/toastdriven/itty/)
![Alt text](/images/python-frame/6.jpg)  
itty.py是一个小实验，是受[Sinatra](http://www.sinatrarb.com/intro-zh.html)的影响而尝试实现的一个微型框架，它刚好够用，没有额外的东西了。

**当前支持**：

- 路由
- 内容类型
- 基本响应
- HTTP状态码
- URL参数
- 支持基本的GET/POST/PUT/DELETE
- 用户可定义的错误处理器
- 重定向
- 文件上传
- 报头
- 静态媒体储存

当心！如果你是要找一个久经考验的，企业级框架，你就来错地方了。但是它确实很有趣。 
###[Klein](http://klein.readthedocs.org/en/latest/)
![Alt text](/images/python-frame/7.jpg)  
Klein是一个使用Python来开发可用于生产环境web服务的微型框架。它基于使用非常广泛且经过良好测试的组件，比如Werkzeug和Twisted，以及近乎完全的测试覆盖率。你可以阅读[这篇文章](http://dreid.org/programming/2012/03/28/klein-a-twisted.web-microframework/)来查看介绍。（也许有点过时了）

###[Morepath](http://morepath.readthedocs.org/en/latest/)
![Alt text](/images/python-frame/8.jpg)  
Morepath是具有强大的能力的Python 微型web框架。Morepath是一个Python WSGI微型框架。他使用路由，但是是针对模型的路由。Morepath是一个模型驱动，灵活的框架，这使得它富有表达力。这里有篇[文章](http://blog.startifact.com/posts/on-the-morepath.html)，关于Morepath的一些细节和建议。

###[ObjectWeb](https://github.com/aisola/ObjectWeb)
![Alt text](/images/python-frame/9.jpg)  
ObjectWeb 是一个快速，极简的纯Python web框架，不依赖任何的第三方库。它围绕Python进行设计，因为起初想要把它当做面向对象的编程语言来使用。ObjectWeb支持CGI和WSGI标准，而且有一个内建的开发服务器。我觉得它是由这个[家伙](http://abram.isola.mn/)设计的

###[Pecan](http://pecanpy.org/)
![Alt text](/images/python-frame/10.jpg)  
创造Pecan是为了填补Python web框架世界的一个空缺——一个提供object-dispatch方式路由的超轻量级的框架。Pecan的目标并不是要成为一个“全栈”框架，因此没有支持一些额外的功能，比如session或是数据库 。相反，Pecan专注于HTTP本身。

###[Pyramid](http://www.pylonsproject.org/)
![Alt text](/images/python-frame/11.jpg)  
Pyramid是一款非常通用的开源web框架。作为一个框架，它的首要任务是让开发者创建web应用变得简单。web应用的类型并不重要，可以是一个电子表单、一个企业内部网或者是一个社交平台。Pyramid非常通用，可以在各种各样的情况下使用它。


通过阅读/观看SixFeetUp上Caliy的这个[教程](http://www.sixfeetup.com/blog/intro-to-the-python-framework-pyramid-and-a-sample-app)，你可以学到更多关于Pyramid的东西。

###[Tornado](http://www.tornadoweb.org/en/stable/)
![Alt text](/images/python-frame/12.jpg)  
Tornado是一个Python web框架，而且是一个异步网络库，最初是为 FriendFeed开发的。通过使用非阻塞I/O，Tornado可以处理数以万计打开的链接，这使它成为长轮询、WebSocket和其他需要为用户提供长连接的应用的理想选择。

Thomas Allen写了一个简单的[教程](http://oinksoft.com/blog/view/3/)，关于Tornado是如何工作的以及如何创建一个简单的静态页面。

###[web.py](http://webpy.org/)
![Alt text](/images/python-frame/13.jpg)  
web.py是一个Python 的web框架，既简单，有强大。web.py处于公有域内，你可以处于任何目的去使用它，没有限制。你可以看[Lucas’s Kauffman](http://cloud101.eu/blog/2012/04/24/python-for-the-web-with-webpy/)博客上的指导文章以及关于它和Django的比较（好吧，我认为我们不能管这叫做比较）。

###[Wheezy Web](http://pythonhosted.org/wheezy.web/)
![Alt text](/images/python-frame/14.jpg)  
一个轻量级、高性能、高并发的WSGI web框架，具备创建现代，高效网络应用的关键功能。这里有一篇来自Andriy Kornatskyy，关于Wheezy的[介绍](http://mindref.blogspot.com/2012/10/wheezy-web-introduction.html)


##Python的最小Web框架

我想，有件事还是值得一提，这些框架实际上都是极小的。因此，像web2py和Django这些框架都不会出现在这个列表中。我欢迎大家反馈，审查各个框架，我同时想知道大家对一份全尺寸web框架的列表是否有兴趣？（这将会包括Django）


我把这些框架放在一起，而且我无意间发现了一些很好的文章——其中一个是一些Python web框架的基准测试，这些框架大多数都在本文中列出了。这个基准测试是由Faruk Akgul所完成的，可以点击这个[链接](http://faruk.akgul.org/blog/python-web-frameworks-benchmark/)查看


Python的官方页面同样也有专门关于web框架的章节，也许你也想[读一下](https://wiki.python.org/moin/WebFrameworks)

下一个是什么？Ruby或者PHP? 

photo by [francois](http://www.flickr.com/photos/frenchy/)
