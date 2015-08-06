---
layout: post
title: "Python装饰器笔记"
category: Python
tags: [Python,装饰器]
image:
  feature: article.jpg
comments: True
share: true
---

#一.函数装饰器

##1.从Python内层函数说起

首先我们来探讨一下这篇文章所讲的内容[Inner Functions - What Are They Good For?](https://realpython.com/blog/python/inner-functions-what-are-they-good-for/)（[中文版](http://python.jobbole.com/81679/)）

###使用内层函数的三个好处

- 封装
- 贯彻DRY原则
- 闭包和工厂函数

1.封装

```python
def outer(num1):
    def inner_increment(num1):  # hidden from outer code
        return num1 + 1
    num2 = inner_increment(num1)
    print(num1, num2)
 
inner_increment(10) ＃不能正确运行
# outer(10) ＃可以正常运行
```

这样把内层函数从全局作用域隐藏起来，不能直接调用。

>使用这种设计模式的一个主要优势在于：在外部函数中对全部参数执行了检查，你可以在内部函数中跳过全部的检查过程。


2.贯彻DRY原则

>比如，你可能写了一个函数用来处理文件，并且你希望它既可以接受一个打开文件对象或是一个文件名：


```python
def process(file_name):
    def do_stuff(file_process):
        for line in file_process:
            print(line)
    if isinstance(file_name, str):
        with open(file_name, 'r') as f:
            do_stuff(f)
    else:
        do_stuff(file_name)
```


**3.闭包和工厂函数**

>闭包无非是使内层函数在调用时记住它当前环境的状态。初学者经常认为闭包就是内层函数，而且实际上它是由内层函数导致的。闭包在栈上“封闭”了局部变量，使其在栈创建执行结束后仍然存在。

```python
def generate_power(number):
   
# define the inner function ...
    def nth_power(power):
        return number ** power
    
# ... which is returned by the factory function
    return nth_power
```

```
>>raise_two = generate_power(2)

>>print(raise_two(7))
    
128
```

外层函数接受一个参数number=2，然后生成一个**nth_power()**函数，该函数只接受一个单一的参数**power**，其中包含**number＝2**

返回的函数被赋值给变量`raise_two`，我们可以通过`raise_two`来调用函数并传递变量。

>换句话说，闭包函数“初始化”了nth_power()函数并将其返回。现在无论你何时调用这个新返回的函数，它都会去查看其私有的快照，也就是包含**number=2**的那一个。


##2.装饰器


「装饰器」是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有**插入日志、性能测试、事务处理**等


装饰器其实就是一个工厂函数，它接受一个函数为参数，然后返回一个新函数，**其闭包中包含了原函数**

###1.简单装饰器

```python
def deco(func):
    def wrapper():
        print "start"
        func() #调用函数
        print "end"
    return wrapper

@deco
def myfun():
    print "run"

myfun()
```

由于装饰器函数返回的是原函数的闭包wrapper，实际上被装饰后的函数就是wrapper，其运行方式就和wrapper一样。

相当于
`myfun=deco(myfun)`



###2.装饰一个需要传递参数的函数

```python
def deco(func):
    def wrapper(param):
        print "start"
        func(param)
        print "end"
    return wrapper


@deco
def myfun(param):
    print "run with param %s"%(param)


myfun("something")
```

这种情况下，仍然返回wrapper，但是这个wrapper可以接受一个参数，因此这样的装饰器只能作用于接受一个参数的函数

###3.装饰任意参数的函数

```python
def deco(func):
    def warpper(*args,**kw):
        print "start"
        func(*args,**kw)
        print "end"
    return warpper


@deco
def myfun1(param1):
    print "run with param %s"%(param1)

@deco
def myfun2(param1,param2):
    print "run with param %s and %s"%(param1,param2)

myfun1("something")
myfun2("something","otherthing")

# start
# run with param something
# end
# start
# run with param something and otherthing
# end
```

两个函数可以被同样一个装饰器所装饰

###4.带参数的装饰器

装饰器接受一个函数作为参数，这个毋庸置疑。但是有时候我们需要装饰器接受另外的参数。此时需要再加一层函数，实际上是定义了一个生成装饰器的工厂函数，调用它，搭配需要的参数，来返回合适的装饰器。

```python
def log(text):
    def deco(func):
        def wrapper(*args,**kw):
            print text
            func(*args,**kw)
            print text + " again"
        return wrapper
    return deco


@log("hello")
def myfun(message):
    print message


myfun("world")

# hello
# world
# hello again
```

这里分两步

-  `log=log("hello")`，把返回的deco函数赋值给**log**，此时log相当于其包含**text＝“hello”**的闭包
-  `myfun=log(myfun)`，相当于把myfun传入了deco函数，并且返回wrapper，并赋值给myfun，此时myfun相当于其装饰后的闭包。


整体来看是`myfun=log("hello")(myfun)`


###5.[装饰器带*类*参数](http://blog.csdn.net/dreamcoding/article/details/8611578)

```python
# -*- coding:gbk -*-  
'''''示例8: 装饰器带类参数'''  
  
class locker:  
    def __init__(self):  
        print("locker.__init__() should be not called.")  
         
    @staticmethod  
    def acquire():  
        print("locker.acquire() called.（这是静态方法）")  
         
    @staticmethod  
    def release():  
        print("  locker.release() called.（不需要对象实例）")  
  
def deco(cls):  
    '''cls 必须实现acquire和release静态方法'''  
    def _deco(func):  
        def __deco():  
            print("before %s called [%s]." % (func.__name__, cls))  
            cls.acquire()  
            try:  
                return func()  
            finally:  
                cls.release()  
        return __deco  
    return _deco  
 
@deco(locker)  
def myfunc():  
    print(" myfunc() called.")  
  
myfunc()  
myfunc()  # -*- coding:gbk -*-  
'''''示例8: 装饰器带类参数'''  
  
class locker:  
    def __init__(self):  
        print("locker.__init__() should be not called.")  
         
    @staticmethod  
    def acquire():  
        print("locker.acquire() called.（这是静态方法）")  
         
    @staticmethod  
    def release():  
        print("  locker.release() called.（不需要对象实例）")  
  
def deco(cls):  
    '''''cls 必须实现acquire和release静态方法'''  
    def _deco(func):  
        def __deco():  
            print("before %s called [%s]." % (func.__name__, cls))  
            cls.acquire()  
            try:  
                return func()  
            finally:  
                cls.release()  
        return __deco  
    return _deco  
 
@deco(locker)  
def myfunc():  
    print(" myfunc() called.")  
  
myfunc()  
myfunc()  
```

####关于wrapper的返回值

上面的代码中，我们的wrapper函数都没有返回值，而是在wrapper中直接调用了func函数，这么做的目的是要在函数运行前后打印一些字符串。而func函数本事也只是打印字符串而已。

但是这么做有时会违背func函数的初衷，比如func函数确实是需要返回值的，那么其装饰后的函数wrapper也应该把值返回。

我们看这样一段函数：

```python
def deco(func):
    def warpper(*args,**kw):
        print "start"
        func(*args,**kw)#直接调用，无返回值
        print "end"
    return warpper

@deco
def myfun(param):
    return 2+param
    
sum=myfun(2) #期望纪录返回值并打印
print sum
```

结果，并没有返回值

```
>>
start
end
None
```

因此我们需要wrapper把函数结果返回:

```python
def deco(func):
    def warpper(*args,**kw):
        print "start"
        result=func(*args,**kw)#纪录结果
        print "end"
        return result #返回
    return warpper
@deco
def myfun(param):
    return 2**param


sum=myfun(2) #这里其实是sum＝result
print sum
```

当然，如果不是为了在func前后打印字符串，也可以把func直接返回




####一个实际例子：统计函数执行时间

```python
from time import time,sleep
def timer(func):
    def warpper(*args,**kw):
        tic=time()
        result=func(*args,**kw)
        toc=time()
        print "%f seconds has passed"%(toc-tic)
        return result
    return warpper

@timer
def myfun():
    sleep(2)
    return "end"

print myfun()

# 2.005432 seconds has passed
# end
```



####关于装饰器装饰过程中函数名称的变化

当装饰器装饰函数并返回wrapper后，原本myfun的`__name__`就改变了

```python
from time import time,sleep

def timer(func):
    def warpper(*args,**kw):
        tic=time()
        result=func(*args,**kw)
        toc=time()
        print func.__name__
        print "%f seconds has passed"%(toc-tic)
        return result
    return warpper

@timer
def myfun():
    sleep(2)
    return "end"

myfun()
print myfun.__name__ #wrapper

# myfun
# 2.003399 seconds has passed
# warpper
```

这样对于一些依赖函数名的功能就会失效，而且也不太符合逻辑，毕竟wrapper对于我们只是一个中间产物

```python
from time import time,sleep
import functools

def timer(func):
    @functools.wraps(func)
    def warpper(*args,**kw):
        tic=time()
        result=func(*args,**kw)
        toc=time()
        print func.__name__
        print "%f seconds has passed"%(toc-tic)
        return result
    return warpper


@timer
def myfun():
    sleep(2)
    return "end"

myfun()
print myfun.__name__ #wrapper

# myfun
# 2.003737 seconds has passed
# myfun
```

导入模块`import functools`，并且用`@functools.wraps(func)`装饰wrapper即可

##3.Flask中的`@app.route()`装饰器
 
[Things which aren't magic - Flask and @app.route - Part 1](http://ains.co/blog/things-which-arent-magic-flask-part-1.html)
[Things which aren't magic - Flask and @app.route - Part 2](http://ains.co/blog/things-which-arent-magic-flask-part-2.html) 

```python
class NotFlask():
    def route(self, route_str):
        def decorator(f):
            return f
 
        return decorator
 
app = NotFlask()
 
@app.route("/")
def hello():
    return "Hello World!"
```

route是NotFlask类的一个方法，并且其实际上是一个装饰器工厂，这里我们并没有装饰我们的函数，装饰器仅仅返回了函数的引用而没有装饰它。

```python
class NotFlask():
    def __init__(self):
        self.routes = {}
 
    def route(self, route_str):
        def decorator(f):
            self.routes[route_str] = f
            return f
 
        return decorator
 
app = NotFlask()
 
@app.route("/")
def hello():
    return "Hello World!"
```

现在给装饰器初始化一个字典，在我们传入参数生产装饰器route的时候，把函数存入字典响应位置，key为url字符串，value为相应函数。


不过此时，我们并不能访问这个内部的视图函数，我们需要一个方法来获取相应的视图函数。


```python
class NotFlask():
    def __init__(self):
        self.routes = {}
 
    def route(self, route_str):
        def decorator(f):
            self.routes[route_str] = f
            return f
 
        return decorator
 
    def serve(self, path):
        view_function = self.routes.get(path)#获取相应函数
        if view_function:
            return view_function()#返回函数
        else:
            raise ValueError('Route "{}"" has not been registered'.format(path))
 
app = NotFlask()
 
@app.route("/")
def hello():
    return "Hello World!"
```

然后我们可以这样，通过url字符串来访问相应的视图函数

```python
app = NotFlask()
 
@app.route("/")
def hello():
    return "Hello World!"
 
print app.serve("/")

#>>Hello World!

```

####小结

Flask路由装饰器的主要功能，就是绑定url到相应的函数。
（如何访问视图函数其实是HTTP服务器的一部分）

--------
当然，目前的url绑定还太死板，我们需要url能够加入可变参数

下面我们要实现从url中识别出参数

```
app = Flask(__name__)
 
@app.route("/hello/<username>")
def hello_user(username):
    return "Hello {}!".format(username)
```

首先我们要利用命名捕获组，从url中识别参数

```python
route_regex = re.compile(r'^/hello/(?P<username>.+)$')
match = route_regex.match("/hello/ains")
 
print match.groupdict()
```

当然，我们需要一个方法来把输入的url转化为相应的正则表达式

```python
def build_route_pattern(route):
    route_regex = re.sub(r'(<\w+>)', r'(?P\1.+)', route)
    return re.compile("^{}$".format(route_regex))
 
print build_route_pattern('/hello/<username>')
```


```python
class NotFlask():
    def __init__(self):
        self.routes = []

    # Here's our build_route_pattern we made earlier
    @staticmethod
    def build_route_pattern(route):
        route_regex = re.sub(r'(<\w+>)', r'(?P\1.+)', route)
        return re.compile("^{}$".format(route_regex))

    def route(self, route_str):
        def decorator(f):
            # Instead of inserting into a dictionary,
            # We'll append the tuple to our route list
            route_pattern = self.build_route_pattern(route_str)
            self.routes.append((route_pattern, f))

            return f

        return decorator
```

与之前的代码不同，字典被移除了，取而代之的是一个列表，然后我们把生成的正则表达式和相应的函数作为元组放到列表里。


同样，我们需要一个方法来返回视图函数，当然，还有捕获匹配组的字典，我们需要它来传递正确的参数

```python
def get_route_match(path):
    for route_pattern, view_function in self.routes:
        m = route_pattern.match(path)
        if m:
           return m.groupdict(), view_function
 
    return None
```

最终结果：

```python
class NotFlask():
    def __init__(self):
        self.routes = []

    @staticmethod
    def build_route_pattern(route):
        route_regex = re.sub(r'(<\w+>)', r'(?P\1.+)', route)
        return re.compile("^{}$".format(route_regex))

    def route(self, route_str):
        def decorator(f):
            route_pattern = self.build_route_pattern(route_str)
            self.routes.append((route_pattern, f))

            return f

        return decorator

    def get_route_match(self, path):
        for route_pattern, view_function in self.routes:
            m = route_pattern.match(path)
            if m:
                return m.groupdict(), view_function

        return None

    def serve(self, path):
        #查找和path匹配的视图函数以及捕获组字典
        route_match = self.get_route_match(path)
        if route_match:
            kwargs, view_function = route_match
            return view_function(**kwargs)#捕获组字典作为函数参数
        else:
            raise ValueError('Route "{}"" has not been registered'.format(path))
```

使用方法：

```python
app = NotFlask()
 
@app.route("/hello/<username>")
def hello_user(username):
    return "Hello {}!".format(username)
 
print app.serve("/hello/ains")
```

```
>>Hello ains!
```

####小结

装饰阶段：
- 装饰器工厂route接受url字符串，生成一个合适的装饰器
- 装饰器装饰视图函数，生成url字符串对应的正则表达式模板，连同视图函数组成元组，存放在列表中。然后把函数返回。

调用阶段：
- app.serve并传入url的时候，首先在列表中查找，依次进行匹配，是否有符合该模式的路径和视图函数
- 有则返回相应获取捕获组字典和视图函数
- 将字典作为参数，**返回该视图函数的运行结果**。


------------

**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)