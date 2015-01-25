---
layout: post
title: "flask+Bootstrap3构建web应用小结"
category: Python
tags: [Python,Bootstrap,flask,web]
image:
  feature: article.jpg
comments: True
share: true

---


###1.配置虚拟环境
1.	安装`virtualenv`
2. 新建文件夹 `mkdir scorecalcu4ME`
3. 创建新的虚拟环境

```python
	cd scorecalcu4ME
	virtualenv workshop
```
4. 激活虚拟环境
`source workshop/Scripts/activate`
![Alt text](./1422103103053.png)


5. 安装所需的包（按需安装）

```python
pip install flask
pip install flask-login
pip install sqlalchemy
pip install flask-sqlalchemy
pip install sqlalchemy-migrate
pip install flask-wtf

```
###2.文件目录
1. 创建我们需要的目录结构

```python
	mkdir app 
	mkdir app/static #存放静态文件（css,js等）
	mkdir app/templates #存放模板文件（html）
	mkdir temp #存放临时文件
```

2. 说明

	![Alt text](./1422001905720.png)
	- `app`文件存放我们的falsk应用的主要脚本
		![Alt text](./1422002011404.png)
		- `app/static` 存放所有的静态文件，包括`css`，`js`代码，`图片`以及其他`插件`或者`文件`。
		- `app/template` 存放`html`文件
	- `workshop`文件夹是我们新建的虚拟环境
	- `temp`用来存放一些临时文件

###3.配置及初始化
1. 创建配置文件[config.py](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/config.py)
`config.py` 文件包含我们所需的各种设置，比如数据库，表单，文件夹的位置等等。把设置参数写在这里的一个明显好处是**调用和管理**非常方便。
例如：
```python
	#config.py
    UPLOAD_FOLDER = basedir+'/uploads/' #should use basedir
```
	这里我们设置了上传文件夹的路径。当然还有很多配置没有显示出来，你可以查看全部代码([config.py](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/config.py))
稍后我们会在这个文件里面配置数据库，表单等设置。

	当我们需要使用该文件的参数时，只需要调用`appME.config['UPLOAD_FOLDER']`就可以获取配置文件中`UPLOAD_FOLDERD`的值。

2. 配置数据库([config.py](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/config.py))
这里我们使用[Flask-SQLAlchemy](https://pythonhosted.org/Flask-SQLAlchemy/)来进行数据库的操作。

```python
import os
basedir = os.path.abspath(os.path.dirname(__file__))
 
SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(basedir, 'app.db')
SQLALCHEMY_MIGRATE_REPO = os.path.join(basedir, 'db_repository')
```
选择SQLite作为我们的数据库，配置两个参数，一个是数据库的路径，一个是数据库迁移仓库。
**此时，我们只是设定了数据库的位置和迁移仓库，但是我们实际上并没有创建一个数据库，稍后我们将编写创建[`db_create.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/db_create.py)，迁移脚本[`db_migrate.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/db_migrate.py)以及编写我们的模型文件[`models.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/app/models.py)**

3. 创建[`__init__.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/app/__init__.py)文件


```python
	#coding:utf-8
from flask import Flask# 导入Flask模块
from flask.ext.sqlalchemy import SQLAlchemy
from flask.ext.login import LoginManager
import os 
#设定常用目录的路径
__StaticDir__=os.path.abspath(os.path.dirname(__file__))+"/static/" 
__ExcelDir__=__StaticDir__+"/excel/" 

appME=Flask(__name__) #初始化应用程序对象
appME.config.from_object('config')#配置文件使用config.py

#初始化数据库
db = SQLAlchemy(app)


#初始化flask-login
lm = LoginManager()
lm.setup_app(appME)

from app import views,models#导入视图和模型

```
**至此，我们已经完成了基本的配置和初始化，而我们的应用名字就叫做*appME*，相关的代码如下**
- [config.py](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/config.py)
- [`__init__.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/app/__init__.py)

但是实际上，我们的web应用并不能运行，因为我们还没有：
- 创建数据库
- 创建模型
- 创建视图


###4.创建和迁移数据库（[`db_create.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/db_create.py)&&[`db_migrate.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/db_migrate.py)）

这里并非我们的重点，可以直接使用这两个现成的脚本，它们均来自[The Flask Mega-Tutorial, Part IV: Database](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database)
- 运行`db_create.py`就可以创建数据库，然后会生成一个`app.db`文件，可以修改参数`SQLALCHEMY_DATABASE_URI` 来改变。

	
	
***TODO***
- 当需要对数据库进行修改的时候，可以运行`db_migrate.py`，则会对数据库进行自动更新。
	`SQLAlchemy-migrate`通过对比数据库的结构（从`app.db`文件读取）和models结构（从`app/models.py`文件读取）的方式来创建迁移任务。
	
	这里需要注意的是，如果我们仅仅修改了`models.py`或是使用SQLite Expert进行了可视化的修改，直接运行`db_migrate.py` 可能会报错，这时我们需要确认`models.py`和SQLite Expert进行了相同的修改。比如，我在`models.py`修改了某个表的某个项，同样的，需要使用SQLite Expert对数据库进行相应的修改。



###5.创建模型（[`models.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/app/models.py)）

我们希望用flask创建一个[MVC](http://baike.baidu.com/item/MVC%E6%A1%86%E6%9E%B6?from_id=85990&type=syn&fromtitle=MVC&fr=aladdin)框架的web应用，这里的M就是我们所说的模型，它包含了数据库的处理，所以此处我们将创建我们所需要的几个表。

根据[在 Flask 中使用 SQLAlchemy¶](http://dormousehole.readthedocs.org/en/latest/patterns/sqlalchemy.html)的说明，我们按照如下的方式来创建我们的数据库模型。

```python
from app import db
ROLE_USER = 0
ROLE_ADMIN = 1
class User(db.Model):
    id = db.Column(db.Integer, primary_key = True)
    name = db.Column(db.String(64), index = True)
    campID = db.Column(db.String(120), index = True, unique = True)
    password= db.Column(db.String(120), index = True)
    grade= db.Column(db.String(64),index = True)
    role = db.Column(db.SmallInteger, default = ROLE_USER)
    score_items = db.relationship('Score_items', backref = 'student', lazy = 'dynamic')
    public_items= db.relationship('Excelmap', backref = 'teacher', lazy = 'dynamic')
    score=db.Column(db.Float,default=0.0)

	def __repr__(self):
        return '<User %r>' % (self.name)

....
```

这里截取一段代码分析一下，定义一个User类，则可以创建一个User表，它包含了几个字段。
查看源代码可以看到，在`models.py`中定义了一些操作数据库的`类方法`，这些方法应该逐渐转移到另外一个文件中去，尽量保持模型文件的整洁。

创建`models.py` 文件后，运行`db_migrate.py`，则该脚本会自动对比`app.db`和模型文件的差异并更新`app.db`。

###6.创建视图（[`views.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/app/views.py)）

>**flask是通过`视图函数`来响应访问请求的**

我们把所有的视图函数都组织在`views.py`文件中，构成MVC的V。


在我们的`views.py`导入了非常多的模块，这里简单的介绍一下如何使用视图函数来返回一个页面。

```python
@appME.route('/test',methods=["POST", "GET"])
def test():
    # return render_template("test.html")
    return "hello world"
```



 `@appME.route('/test',methods=["POST", "GET"])`是一个`路由`，可以把对`/test`的请求映射到函数`test()`上，我们可以这样理解，当用户访问`/test`的时候，通过路由，实际上的调用了这里的`test()`函数，然后我们可以返回相应的页面，或是完成一些其他的处理操作。
这个函数看上去比较奇怪，实际上它是一个装饰器，如果希望了解更多，请看扩展阅读。

**至此，我们的flask实际上已经可以运行了，下面就让我们看一下运行的结果吧。**

#####扩展阅读
- [这不是魔法：Flask和@app.route](http://python.jobbole.com/80956/)
- [廖雪峰的官方网站-->Python教程-->装饰器](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386819879946007bbf6ad052463ab18034f0254bf355000)

###7.试运行
创建一个[`run.py`](https://github.com/hanxiaomax/scorecalcu4ME/blob/master/run.py)文件



```python
#!workshop/Scripts/python
from app import appME
if __name__ == '__main__':
    appME.run(debug=True)

```
启动：
![Alt text](./1422102904678.png)

在浏览器访问我们的相应页面：
![Alt text](./1422102683983.png)

###8.模板

上一章中我们提到了模板渲染函数`render_template()`，它接受一个`.html`文件作为参数，而实际上，并不一定如此，它可以直接接受一个字符串

- 首先我们导入了一个`render_template`，它用来渲染模板
