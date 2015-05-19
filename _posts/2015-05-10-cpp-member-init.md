---
layout: post
title: "C++成员变量初始化的几点思考"
category: cpp
tags: [C/C++,笔记]
image:
  feature: article.jpg
comments: True
share: true
---



最近在学习QT，使用Qt 设计设计的界面，会多一个中间层Ui::MainWindow，它被用来初始化界面，但是界面的逻辑需要再创建一个类来实现。一般这个类的命名是不带前缀的同名类，即MainWindow，《C++ GUI Qt 4 编程》书中介绍的方法是让这个类直接继承Ui::MainWindow和Ui::MainWindow的父类。在研究这个问题的时候，发现如果在继承列表里面调整顺序，则编译不通过。网上搜索发现，约定必须要把QT类放在第一位，既然是约定就不说了。不过在网上提了一个相关的问题，[c++多重继承的顺序应该如何写](http://segmentfault.com/q/1010000002741153)。其中答案给我很大的帮助，作者说到，一般并不会直接继承。而是创建一个Ui::MainWindow类型的成员变量。在尝试创建这个成员变量的时候遇到了一些问题，在此做一个总结。



##成员变量可以以以下三种形式存在
1. 对象
2. 引用
3. 指针


###对于引用和指针
引用比指针更安全，不存在空的引用，但是指针可能为空，指针在使用之前一定要检查是不是空。
两者在调用成员的时候也是不一样的，对象或者引用可以使用`.`，而指针要使用`->`。


所以一般来讲使用引用要好一些，但是也正是因为引用不能为空，所以在构造函数里面必须要初始化对象或者引用。

对于引用，采用以下形式。注意，这里是把ui绑定到_UI，后者可以是引用或者是一个对象，此时前者是引用或者对象都可以。但是如果后者是一个对象，而前者是一个引用，则会出现警告：**binding reference member 'ui' to stack allocated parameter '_UI'**，一般常见的还是两者均为引用。

```cpp
MainWindow::MainWindow(Ui::MainWindow &_UI,QWidget *parent)
:QMainWindow(parent),ui(_UI)
{
    ui.setupUi(this);
}
```

请注意下面这种形式，比上面少了一个参数，这里想要通过`Ui::MainWindow()`为ui初始化，创建的是一个无名临时变量，是不可以被绑定到非const类型的左值的。会出现错误：
**non-const lvalue reference to type ‘
Ui::MainWindow ' cannot bind to a temporary of type '
Ui::MainWindow '**。
(VS之后提示一个warring)  
同时由于成员变量ui是一个引用或对象，那么不能使用`new Ui::MainWindow`这种形式。
(`new` 返回的是指针)，而应该使用是值初始化的形式`Ui::MainWindow()`（特例是对象声明了一个private的拷贝构造函数）

```cpp
MainWindow::MainWindow(QWidget *parent)
:QMainWindow(parent),ui(Ui::MainWindow())
{
    ui.setupUi(this);
}
```


或者在外面初始化为有名的变量后传递进来才行。

```cpp
int main(int argc,char *argv[])
{
    QApplication app(argc,argv);
    Ui::MainWindow _UI = Ui::MainWindow();
    MainWindow *mainwindow = new MainWindow(_UI);//这里传进构造函数
    mainwindow->show();
    app.exec();
}
```

还有的对象可以在类内完成初始化，比如这里ui，在声明的时候初始化就完成了`Ui::mainformClass ui;`，因此这里初始化列表实际上不需要包含ui,需要默认构造函数支持

对于指针
```cpp
Ui::MainWindow* ui;
```

```cpp
MainWindow::MainWindow(QWidget *parent)
:QMainWindow(parent),ui(new Ui::MainWindow) // new 返回的是指针
{
    ui->setupUi(this);
}
```

这里可以直接在初始化列表里面使用`new Ui::MainWindow`来进行初始化，构造函数少一个参数。
也不需要在外面把有名变量传递进行做初始化

```cpp
int main(int argc,char *argv[])
{
    QApplication app(argc,argv);
    MainWindow *mainwindow = new MainWindow;
    mainwindow->show();
    app.exec();
}
```

现在有一点不明白，成员变量使用引用和对象形式有什么优劣，目前所了解的一个缺点就是上面所说的，如果成员变量是一个对象，那么构造函数的参数也要是一个对象，如果是引用就会出现警告。那如果参数是一个引用，就类似传值的方式了，似乎会多创建一个对象副本。而如果是引用的方式则为传址的方式，似乎更具优势。而如果成员变量是一个对象，那么是可以在初始化列表里面直接初始化（这里涉及到绑定一个临时变量的问题），可以少一个参数。

- 引用适合传值和返回时使用（传址方式，而且避免了临时变量的问题）
- 对象适合在类内使用（节省参数）
- 指针个人觉得尽量少使用，但是有些时候，不需要把所有对象初始化，因为有些可能用不到。或者不想要在构造函数的初始化列表里面初始化(是否在构造函数内部把它们赋值为NULL更好？)。不过指针在使用之前必须要考察是否为空。而且声明的时候也会赋值一个空指针



参考资料：

- [C++中引用和指针的区别 - Listening_music的专栏](http://blog.csdn.net/listening_music/article/details/6921608)



-----------------------
**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)
