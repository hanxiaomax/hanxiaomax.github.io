---
layout: post
title: "Python PyH模块中文文档"
category: trans
tags: [Python,PyH,文档]
image:
  feature: article.jpg
comments: True
share: true
---

*本文翻译自[UserManual](https://code.google.com/p/pyh/wiki/UserManual)*

**仅作交流和分享之用，未经许可，禁止转载！**

---------------------------------

[GitHub维基百科](https://github.com/hanxiaomax/pyh/wiki)


# 模块介绍 #

Pyh 是一个强大且简约的python模块，你可以使用它在python程序中生成HTML内容。在python代码中手写HTML代码非常乏味并且使代码可读性变得非常糟糕。而且，当你尝试要去看一下HTML源码的时候，可读性同样很差。PyH为这一切提供了非常不错的解决方案。它让你像编写GUI一样，编写你的网页！

## 功能特性 ##
  * 自动格式化HTML标签
  * 高度可定制
  * 完全识别CSS和Javascript
  * 自动闭合标签
  * 面向对象的HTML编写方式

# 如何安装PyH #
从这个[下载页面](http://code.google.com/p/pyh/downloads/list)下载（译注：原链接需要代理，可以在本人托管的[GitHub地址](https://github.com/hanxiaomax/pyh)下载）或者直接下载 [PyH-0.1.tar.gz](http://pyh.googlecode.com/files/PyH-0.1.tar.gz)（译注：同样需要代理）解压到你的工作目录或是python目录，通常是 `/usr/lib/pythonX.X/site-packages`或者是其他任意能够被 `$PYTHONPATH`识别的路径:

```bash
$ wget http://pyh.googlecode.com/files/PyH-0.1.tar.gz
$ tar xvzf PyH-0.1.tar.gz
$ cd PyH-0.1
$ sudo python setup.py install
```
如果你没有root权限，可以把 `PyH-0.1/pyh.py`文件拷贝到你的项目目录下。如果你正在使用一个基于RPM的发行版系统，你可以使用rpm二进制包，并通过如下命令来安装：

```bash
$ wget http://pyh.googlecode.com/files/PyH-0.1-1.noarch.rpm
$ sudo rpm -ivh PyH-0.1-1.noarch.rpm
```
### 快速例程 ###

下面这段python代码：

```python
from pyh import *
page = PyH('My wonderful PyH page')
page.addCSS('myStylesheet1.css', 'myStylesheet2.css')
page.addJS('myJavascript1.js', 'myJavascript2.js')
page << h1('My big title', cl='center')
page << div(cl='myCSSclass1 myCSSclass2', id='myDiv1') << p('I love PyH!', id='myP1')
mydiv2 = page << div(id='myDiv2')
mydiv2 << h2('A smaller title') + p('Followed by a paragraph.')
page << div(id='myDiv3')
page.myDiv3.attributes['cl'] = 'myCSSclass3'
page.myDiv3 << p('Another paragraph')
page.printOut()
```

会生成这段html代码：


```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>My wonderful PyH page</title>
<link href="myStylesheet1.css" type="text/css" rel="stylesheet" />
<link href="myStylesheet2.css" type="text/css" rel="stylesheet" />
<script src="myJavascript1.js" type="text/javascript"></script>
<script src="myJavascript2.js" type="text/javascript"></script>
</head>
<body>
<h1 class="center">My big title</h1>
<div id="myDiv1" class="myCSSclass1 myCSSclass2">
<p id="myP1">I love PyH!</p>
</div>
<div id="myDiv2">
<h2>A smaller title</h2>
<p>Followed by a paragraph.</p>
</div>
<div id="myDiv3" class="myCSSclass3">
<p>Another paragraph</p>
</div>
</body>
</html>
```

# 基础手册 #

pyh.py可以通过如下方式导入:

```python
>>> from pyh import *
```
## `Tag` 对象 ##

HTML 标签可以通过调用同名函数来生成。HTML 标签
`<tag>` 可以通过 `tag()` 生成:

```python
>>> mydiv = div()
>>> mydiv.render()
'<div></div>'
```

标签的属性，通过函数的关键字参数传入，关键字和标签的属性是同名的，除了`class` 这个属性被替换成了 `cl`. 标签的内容或是子标签可以通过非关键字参数传入:

```python
>>> mydiv = div('My content', cl='myCSSclass1 myCSSclass2', id='myCSSid1')
>>> mydiv.render()
'<div class="myCSSclass1 myCSSclass2" id="myCSSid1">
My content
</div>'
```

其他的标签同样可以作为内容传入:

```python
>>> mydiv = div(p('My paragraph.'), cl='myCSSclass1 myCSSclass2', id='myCSSid1')
>>> mydiv.render()
'<div class="myCSSclass1 myCSSclass2" id="myCSSid1">
<p>My paragraph</p>
</div>'
```

当一个标签对象被创建后，HTML属性可以通过它的`属性`成员来修改，它是一个字典：

```python
>>> mydiv = div()
>>> mydiv.attributes['id'] = 'myCSSid'
>>> mydiv.render()
'<div id="myCSSid"></div>\n'
```

标签可以通过`+`号进行连接：

```python
>>> twoDivs = div() + div()
>>> twoDivs.render()
'<div></div>
<div></div>'
```

标签可以被包含进更高一层的标签中，可以把它作为非关键字参数传入（就像上面介绍的那样），也可以通过`<<`操作符来完成。这个操作符会返回最后被包含的标签：

```python
>>> myDiv = div(id='myTopLevelDiv')
>>> myPar = myDiv << div(id='myInnerDiv') << p(id='myPar') ＃注意此处返回的是p标签
>>> myPar << span('My first span') + span('My second span')
>>> myDiv.render()
'<div id="myTopLevelDiv">
<div id="myInnerDiv">
<p id="myPar">
<span>My first span</span><span>My second span</span>
</p>
</div>
</div>'
```

当一个标签被包含进另一个标签之后，可以把它作为上级标签的成员来进行访问，它的名字就是其`id`属性的值，如果没有指定该值，则`tag_001`就是该类第一个标签的名字，以此类推：

```python
>>> myDiv = div()
>>> myDiv << span(id='myspan')
>>> myDiv.myspan << 'content1'
>>> myDiv << span()
>>> myDiv.span << 'content2'
>>> myDiv << span()
>>> myDiv.span_001 << 'content3'
>>> myDiv.render()
'<div>
<span id="myspan">content1</span>
<span>content2</span>
<span>content3</span>
</div>'
```

## 创建网页 ##
最高层的对象，是`PyH`对象。它创建一个包含CSS和javascript元素的HTML页面。创建一个网页的第一件事是实例化一个`PyH`对象:

```python
from pyh import *
page = PyH('My wonderful PyH page')
```

随后你可以添加你的CSS表盒javascript文件，例如：


```python
page.addCSS('myStylesheet1.css', 'myStylesheet2.css')
page.addJS('myJavascript1.js', 'myJavascript2.js')
```
注意，它们可以在任意时刻通过python脚本添加（例如，根据你的内容，仅添加被用到的脚本和样式表）。你现在可以开始创建你的页面了：

```python
page << h1('My big title!', cl='myCSSclass')
page << div(id='mySubtitleDiv') << h2('My sub-title')
maindiv = page << div(id='myMainDiv')
maindiv << h3('A smaller title') + p('followed by a small paragraph')
maindiv << p('Main paragraph ', style='color:red;') << a('a link', href='http://alink')
maindiv << span('hehe', onclick='myJavaScriptFcn(); return false;')
maindiv << br()
maindiv << img(src='mypicture.jpg')
page << div(id='myFooter') << span('My footer')
```

之后，你可以把HTML页面输出到文件，或是输出到浏览器，如果你的脚本是在服务器上运行的话。

```python
page.printOut()
```

输出的结果是：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>My wonderful PyH page</title>
<link href="myStylesheet1.css" type="text/css" rel="stylesheet" />
<link href="myStylesheet2.css" type="text/css" rel="stylesheet" />
<script src="myJavascript1.js" type="text/javascript"></script>
<script src="myJavascript2.js" type="text/javascript"></script>
</head>
<body>
<h1 class="myCSSclass">My big title!</h1>
<div id="mySubtitleDiv"><h2>My sub-title</h2></div>
<div id="myMainDiv">
<h3>A smaller title</h3>
<p>followed by a small paragraph</p>
<p style="color:red;">Main paragraph <a href="http://alink">a link</a>
<span onclick="myJavaScriptFcn(); return false;">hehe</span><br />
<img src="mypicture.jpg" />
</div>
<div id="myFooter"><span>My footer</span>
</div>
</body>
</html>
```

## 创建表格 ##

你可以充分利用python和pyh的能力来高校的生成HTML表格，创建一个4乘4的表格非常轻松：

```python
page = PyH('My wonderful PyH page')
page << h2('Most compact way to build a 4 by 4 table')
page << table() << tr(td('1') + td('2')) + tr(td('3') + td('4'))
page << h2('Another way to build a 4 by 4 table')
mytab = page << table()
tr1 = mytab << tr()
tr1 << td('1') + td('2')
tr2 = mytab << tr()
tr2 << td('3') + td('4')
page.printOut()
```

上面的代码会生成如下HTML代码（为了简洁，表头被去掉了）

```html
<h2>Most compact way to build a 4 by 4 table</h2>
<table>
<tr>
<td>1</td><td>2</td>
</tr><tr>
<td>3</td><td>4</td>
</tr>
</table>
<h2>Another way to build a 4 by 4 table</h2>
<table>
<tr>
<td>1</td><td>2</td>
</tr><tr>
<td>3</td><td>4</td>
</tr>
</table>
```

现在，如果你想要自动的创建一个大型表格，比如从数据库中创建，你可以这样做：

```python
mytab = page << table()
for i in range(nrows):
    mytr = mytab << tr()
    for j in range(ncols):
        mytr << td('Row %i, column %j' % (i, j))
```

这样可以生成一个i乘j的表格。
