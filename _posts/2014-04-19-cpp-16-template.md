---
layout: post
title: "《C++primer》16章 模板与泛型编程"
category: cpp
tags: [C/C++,answer]
modified: 2014-02-21
image:
  feature: article.jpg
comments: true
share: true
---

泛型编程：独立于任何特定类型的方式编写代码

目录

* Table of Contents
{:toc}

## 模板定义

###函数模板

1. **定义**一个函数模板    
{% highlight c linenos %}
template<typename TYPE>//typename也可以用class代替，typename是较新的标准
int compare(const TYPE &v1,const TYPE &v2)
{
        return v1<v2? -1:1;
}
{% endhighlight %}

2. **使用**一个**函数模板**  
`compare(1,2)；//编译器会自动实例化compare`

3. `inline`函数模板  
要注意的是`inline`关键词在模板参数列表之后  
`template<typename TYPE> inline TYPE fun(const TYPE v1,const TYPE v1);`



###类模板

1. **定义**一个**类模板**  
{% highlight c linenos %}
template<typename TYPE> 
class QUEUE
{
    public:
        QUEUE();//构造函数
        TYPE &front();
        const TYPE &front() const;//重载
        void push (const TYPE &);
        void pop();
        bool empty() const;
    private:
};
{% endhighlight %}
2. **使用**一个**类模板**      

需要注意的是，使用类模板需要显式的指定实参


    QUEUE<int> queue_int //实例化一个QUEUE<int>类型的实例queue_int
    QUEUE<vector<double>> queue_vector_double


###在模板定义内部指定类型

除了定义`数据成员`和`函数成员`以外，类还可以定义`类型成员`，如果我们要在`函数模板`内部使用这样的类型，必须告诉编译器我们正在使用的名字指的是一个类型。

{% highlight c linenos %}
template<class Parm,class U>
Parm fcn(Parm * array,U value)
{
    typename Parm::size_type *p //告诉编译器将成员当做类型
}
{% endhighlight %}

###非类型模板形参

*模板的形参不必都是类型！*

####函数模板


{% highlight c linenos %}
//初始化一个数组为全0
template<class TYPE ,size_t N>
void array_init(TYPE (&parm)[N])
{
    for(size_t i=0;i!=N;++i)
    {
        parm[i]=0;
    }
}
{% endhighlight %}

*编译器将根据数组的具体情况，计算N的值并实例化不同版本的函数*

####类型等价性

对非类型形参，如果求值结果相同，则被认为是等价的


{% highlight c linenos %}
int x[42];
const int sz=40;
int y[sz+2];
array_init(x);//array_init<int,42>
array_init(y);//array_init<int,42>
{% endhighlight %}

###注意
1. 虽然模板对一切类型有效，但是实例化后的结果可能是非法的，所以在编写前总是会假定可能使用的类型。
2. 在编写代码的时候，对实参类型的要求尽可能少是比较有益的
3. 两个重要原则：模板的形参是const引用，函数体中的测试只用<(可以减少对类型的要求)

###发现错误的阶段
1. 编译模板定义本身
2. 编译器检查模板的使用时
3. 实例化时
4. 在链接的时候

##实例化
模板相当于一个蓝图，编译器根据他来产生制定的类或者函数，这一过程称为**实例化**

###类的实例化



















