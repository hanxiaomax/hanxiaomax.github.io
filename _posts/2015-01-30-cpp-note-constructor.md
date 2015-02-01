---
layout: post
title: "C++构造函数笔记"
category: cpp
tags: [C/C++,笔记]
modified: 2015-01-30
image:
  feature: article.jpg
comments: true
share: true
---


##1.基本

1.  与类名相同，没有返回值类型
2. 决定对象初始化的方式，初始化类对象的数据成员
3. 不能被声明为`const`类型，因此在创建一个`const`类型的对象时，只有在构造完成后，才获得`const`属性。

##2.合成的默认构造函数

当**没有定义任何构造函数的时候**，编译器会创造一个构造函数，叫做`合成的默认构造函数`，它按照如下规则初始化数据成员：

- 如果存在[类内初始值]()，用它来初始化
- 否则，默认初始化该成员

```cpp
class Member
{
    //没有构造函数
    private:
        int val;
};
...
Member b //调用默认构造函数
```
```cpp
class Member
{
    public:
        Member()//显式定义一个默认的构造函数，否则无法调用默认构造函数
        Member(int a) : val(a) {}//自定义构造函数
    private:
        int val;
};
...
Member b //调用默认构造函数
```

####默认初始化

在没有给定初始值的情况下，变量将被**默认初始化**，默认值是多少，由变量类型决定，同时，**定义变量的位置也会影响它的值**。（`类型`，`位置`）

- 对于**内置类型**，它的值由**位置**决定。
    - 函数体外-->`0`  
    - 函数体内-->`未定义`（不被初始化），在试图拷贝或访问时会出错
- 每个类各自决定其初始化对象的方式。是否允许不初始化就定义对象，也由类自己决定。                                                   


####不能依赖合成的默认构造函数

1. 编译器只有在没有发现任何构造函数的情况下才会合成默认构造函数，一旦我们定义了其他的构造函数，那么除非再定义一个默认构造函数，否则类将没有默认构造函数。

2. 块中的**内置类型**或**复合类型**（数组，指针）如果默认初始化，则值会为`未定义`，将导致错误。除非类内的这些成员全部被赋予了**类内初始值**。

3. 有时，编译器不能为某些类合成默认的构造函数。
如果类内**包含一个其他类类型的成员**，**而且该成员的类型没有默认构造函数**，则无法初始化。此时我们必须自定义构造函数，否则将没有可用的默认构造函数。（见：必须使用初始化列表--3）


####C++11`=default`

`A()=default`，这样定义一个默认构造函数。

##3.构造函数初始化列表

```cpp
class Member
{
    public:
        Member(int a) : val(a) {}//初始化列表
    private:
        int val;
};
```

- 当某个数据成员被初始化列表忽略时，将以合成的默认构造函数来初始化它。
- 构造函数不应该轻易的覆盖类内初始值，除非新值与原值不同
- 使用**类内初始值**是一个比较好的选择，但是有的编译器不支持  


##4.C++ 必须在类初始化列表中初始化的几种情况  

###1.类成员为`const`类型  
###2.类成员为引用类型  

`const` 和引用类型只能初始化，但是不能赋值。
而初始化列表和函数体内赋值是不一样的。

```cpp
class Example
{
    public:
        Example(int &v) : i(v), p(v), j(v) {}
        void print_val() { cout << "hello:" << i << "  " << j << endl;}
    private:
        const int i;//const
        int p;
        int &j;//引用
};
```


- 初始化
调用拷贝[构造函数](#)创建新对象
- 赋值
调用赋值操作符给已有对象赋值

###3.类成员没有默认构造函数  

`Member` 类有一个显式定义的构造函数，那么编译器不会为其生成一个默认的不带参的构造函数。因此初始化这个对象，就必须要给一个参数。

```cpp
class Member
{
    public:
        //Member(){} 除非在此定义一个默认构造函数
        Member(int a) : val(a) {}
    private:
        int val;
};

class Example
{
    public:
        Example(int v) : p(v), b(v) {}//调用构造函数初始化b
        void print_val() { cout << "hello:" << p << endl;}
    private:
        int p;
        Member b;//没有默认构造函数，如果不进行初始化，则会出现错误
};
```

- `Member b;`**没有默认的构造函数**的话，这里就错误了，除非使用`Example`类初始化列表，对它初始化。如果**有默认构造函数**，则这里是可以的。
- `Example(int v) : p(v){ b(v) }`这种写法本身就是错误的，b不是一个函数。
- `Example(int v) : p(v){ Member b(v) }`这种写法没什么问题，调用了`Member`带一个参数的构造函数，但是这样就是新创建了一个对象，和下面的`Member b`没有关系，因此`Member b`仍然没有初始化（缺少默认构造函数）

###4.派生类必须在其初始化列表中调用基类的构造函数  

```cpp
class Member//基类
{
    public:
        Member(int a) : val(a) {}
    private:
        int val;
};

class Example : public Base//派生类
{
    public:
        A(int v) : p(v), Base(v) {}//调用基类构造函数
        void print_val() { cout << "hello:" << p << endl;}
    private:
        int p;
};


```


###5. 拷贝构造函数
####0.从成员逐一初始化（Memberwise Initialization）说起

```cpp
Example ex1(0);
Example ex2=ex1;//以一个对象作为另一个对象的初值
```
有时候我们会遇到上面的这种情况，以一个对象作为另一个对象的初值。此时，ex1的各个成员变量会一次进行复制到ex2。**这是一种默认的行为，但是并不能保证对所有情况都适用。**

例如：

```cpp
class Example{
    public:
        Example(int x,int y):_x(x),_y(y){
            p=new int[x*y];//分配一块内存，p为指针
        }
}
```

如果我们的`Example`其中包含这样动态内存分配的代码，那么就可能出现问题，试想：


```cpp
int main()
{
    Example ex1(1,2);
    //ex1构造
    {
        Example ex2=ex1;
        
        //ex2析构，销毁了指针p
    }
    //使用ex1
    //ex1析构
}

```
默认的成员逐一初始化，会把`ex1`的各个成员复制到`ex2`，因此`ex2`和`ex1`的`p`指针是相同的，此时由于二者的生命周期可能不同，当其中一个析构后，就会影响另外一个还没有析构的对象，导致其`p`指针所指的内存被释放。

因此我们要改变这种初始化的方法。

ps：[需要拷贝构造函数的三种情况](http://blog.csdn.net/lwbeyond/article/details/6202256)

####1.拷贝构造函数（copy constructor）
拷贝构造函数是构造函数的一种重载形态，它仅接受唯一的参数，即`指向同类的引用`，例如：`Example(const Example &ex)`。不过`const`并不是必须的。

我们可以在拷贝构造函数里面解决上述默认初始化方式造成的困境，即创建一个**独立的副本**。
```cpp
class Example{
    public:
        Example(int x,int y):_x(x),_y(y){
            p=new int[x*y];//分配一块内存，p为指针
        }
        //拷贝构造函数
        Example(const Example &ex):_x(x),_y(y){
            _size=x*y;
            p=new int[x*y];//分配一块内存，p为指针
            for(int i=0;i<_size;i++){
                p[i]=ex.p[i];//创建一个副本
            }
        }
```



####2.重载赋值操作符（copy assignment operator）
上面的方案并非万无一失，对于一个普通的`赋值操作符`，它的功能仅仅是给被赋值对象赋值，而有时，在我们赋值之前，还需要丢弃一些原有的内容。
试想，

```cpp
class Example{
    public:
        Example(int x,int y):_x(x),_y(y){
            p=new int[x*y];//分配一块内存，p为指针
        }
}
...     
Example ex1(0);
Example ex2(0);
Example ex2=ex1;//把ex1赋值给ex2
```
作为一个常规的赋值符，`ex2`的`p`指针会被`ex1`所替换，这将导致ex2的`p`指针之前指向的内存泄露，因此，在p指针被替换前，它之前所指的内存需要被释放。

```cpp
Example &Example :: 
operator=(const Example &ex){
    if(this!=&ex){
        x=ex.x;
        y=ex.y;
        int _size=x*y;
        delete [] p;//丢弃原有内容
        p=new int[_size];//创建副本
        for(int i=0;i<size;i++){
            p[i]=ex.p[i];
        }
    }
    return *this;
}
```

###6.扩展阅读
- [C++拷贝构造函数详解](http://blog.csdn.net/lwbeyond/article/details/6202256)
