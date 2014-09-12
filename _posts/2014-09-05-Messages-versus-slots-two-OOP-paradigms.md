---
layout: post
title: "消息VS槽---两种面向对象范式"
category: trans
tags: [技术翻译,面向对象范式,消息,槽]
image:
  feature: article.jpg
comments: True
share: true
---



本文翻译自[Messages versus slots - two OOP paradigms](http://live.julik.nl/2012/08/messages-versus-slots) 

译者[Lingfeng Ai](http:\\hanxiaomax.github.com) 
**仅做学习交流使用，未经允许，请勿转载**


我之前已经和[oleg](http://blog.oleganza.com/)讨论稿这个问题，不过这个问题还是反复的出现。你看，很多人生Ruby的气，因为它不能这样做：
{% highlight ruby%}
something = lambda do
  # execute things
end
something() # won't work
{% endhighlight %}

而且也不能这样做：

{% highlight ruby%}
meth = some_object.method(:length)
meth(with_argument) # call the meth
{% endhighlight %}

然而，这是因为Ruby是一种消息传递型的语言，这和槽语言是相对的。我倾向于把一个槽语言称之为：假定对象不是别的，仅仅是一个美化版的hash表的这样一种语言。hash表的键因此会是实例变量或者，如果它储存了一个函数的话，键会是“callables”（Python语言这样称呼它）

因此，你也许有一个Car对象，他有如下的键：
` [ Car ] -> [weight, price, drive()]`

这些键全部处于一个命名空间，使用槽s范例工作的语言，通常会具有以下特性：

- 非常容易把一个方法重新绑定到另一个对象上——仅需要复制槽的值
- 你可以在同一个流中同时在实例变量和方法上进行迭代
- 封装只是一种装饰，因为实际上所有的东西仍然在一个表里
- 你可以直接使用变量，因为局部命名空间仍然是一个hash表，表里有变量名的键和可以调用的内容。

因为各种各样的原因，我不喜欢这样的方式。首先，我倾向于把对象看做是接收消息的*演员* 。也就是说，储存在对象里的内部变量，对外界来讲不应该是可见的，至少不应该通过合法的方式可见。想象一下，一个槽语言计算字符串长度的方式。

{% highlight python%}
str.len # is it a magic variable?
str.len() # is it a method?
# or do we need a shitty workaround which will call some kind of __length__?
len(str) # WTF does that do??
{% endhighlight %}

对于变量是否可以调用，这里非常的模棱两可，而且这种模棱两可不能自动被处理，因为这一表达式在槽语言中不明确。

{% highlight python%}
# will it be a function ref?
# or will it be the length itself?
m = str.len
{% endhighlight %}

因此，像Python这样的语言就总是需要显式的调用。这就需要输入括号，或是这样做：
`m.do_stuff.call()`

大多数我们所知的面向对象范例（或半面向对象范例）系统（甚至CLOS，据我所知）是槽系统——IO, Python, Javascript 都基于槽。这产生了一个实质性的好处就是：不需要显示的指明你想要把一个代码块作为值来移动。所以，在JS中你可以这样做：
{% highlight python%}
 // boom! we transplanted a method
 someVar.smartMethod=another.otherMethod;
{% endhighlight %}
其他的方法都是不够优雅的。首先，因为一个方法可以当做值来使用，用来把代码从一个地方移动到另一个地方。你总是需要显示的调用。第二，对象的槽不会区别值和方法，所以你需要额外的询问槽表中的每一个值，它是一个方法还是变量。同样，在槽语言中，你需要指明一个实例变量是不是私有的。（因为通常情况下所有的东西都在一个大hash表中）。

另一方面，我们还有消息传递语言，比如Smalltalk和Ruby。这些语言中，你从一个对象接受到的一切，都是通过方法的调用，不论你愿意不愿意，因为这里存在两个命名空间——一个是实例变量的，另一个是方法的。

你知道，从对象外部访问实例变量是不可以的，而且你知道，所有暴露给外部的东西都是可以调用的，定义上是这么说的。你同样还能获得如下的好处：

- 所有的东西都通过getters 和 setters控制，所以你不要总是操心它们。
- 你可以避免把对象作为一个美化版hash表使用。
- 你获得了很多智能语法捷径
- 把某个属性重构放入getter/setter对，对于用户代码来说不是问题。

消息传递语言同时遵循[UAP](http://en.wikipedia.org/wiki/Uniform_access_principle)

当你实现你下一个编程语言的时候，首先把它设计为一个**消息传递型系统**，然后当直接获取属性的时候，如果你需要提升性能，使用内部不透明的捷径来绕开getter/setter。你可以为很多人节省很多不必要的猜测和输入。
