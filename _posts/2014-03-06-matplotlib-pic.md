---
layout: post
title: "Matplotlib learning note:Lines, bars, and markers"
category: python
tags: [python,matplotlib]
image:
  feature: 2000px-Matplotlib_logo.svg.png
comments: true
share: true
---

目录

* Table of Contents
{:toc}


###1.水平柱状图
[lines bars and markers example code: barh demo.py](http://matplotlib.org/examples/lines_bars_and_markers/barh_demo.html)


<figure>
    <a href="/images/barh_demo.png"> <!--herf是超链接-->
        <img src="/images/barh_demo.png"><!--img标签必须有src属性=“图片位置”-->
    </a>
</figure>


{% highlight python linenos %}
#coding:utf-8
"""
Simple demo of a horizontal bar chart.
"""
import matplotlib.pyplot as plt; plt.rcdefaults()
import numpy as np
import matplotlib.pyplot as plt
# Example data
people = ('Tom', 'Dick', 'Harry', 'Slim', 'Jim')#定义一组数据
y_pos = np.arange(len(people))#arange类似range不过返回的是array
performance = 3 + 10 * np.random.rand(len(people))#随机生成每个人的表现
error = np.random.rand(len(people))

plt.barh(y_pos, performance,height=0.6,xerr=error, align='center', alpha=0.1)
#创建一个horizontal bar
"""barh(bottom, width, height=0.8, left=0, **kwargs)
bottom:底部坐标
width：横向长度
height:柱的宽度
"""
plt.yticks(y_pos, people)#y坐标
plt.xlabel('Performance')#x轴名称
plt.title('How fast do you want to go today?')#表名

plt.show()#显示图片
{% endhighlight %}

*************************
1.`matplotlib.pyplot.rcdefaults()`  
Restore the default rc params. These are not the params loaded by the rc file, but mpl’s internal params. See rc_file_defaults for reloading the default params from the rc file

[info:rcdefaults](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.rcdefaults)

2.`matplotlib.pyplot.barh(bottom, width, height=0.8, left=None, hold=None, **kwargs)`

[info:Make a horizontal bar plot](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.barh)

3.`matplotlib.pyplot.yticks(*args, **kwargs)` 

[info:yticks](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.yticks)

4.`matplotlib.pyplot.xlabel(s, *args, **kwargs)`

[info:xlabel](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.xlabel)

5.`matplotlib.pyplot.title(s, *args, **kwargs)`

[info:title](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.title)


********************************************
1.`np.arange()`

To create sequences of numbers, NumPy provides a function analogous to range that returns arrays instead of lists

2.`np.random.rand()`


********************************************


###2.填充曲线
[lines bars and markers example code: fill demo.py](http://matplotlib.org/examples/lines_bars_and_markers/fill_demo.html)

<figure>
    <a href="/images/1-figure-2.png"> <!--herf是超链接-->
        <img src="/images/1-figure-2.png"><!--img标签必须有src属性=“图片位置”-->
    </a>
</figure>

{% highlight python linenos %}
#coding:utf-8
"""
Simple demo of the fill function.
"""
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 1)#0-1等步长
y = np.sin(4 * np.pi * x) * np.exp(-5 * x)#sin(4*pi*x)*x^-5

plt.fill(x, y, 'r') #使用红色填充填充
plt.grid(True) #打开网格
plt.show()
{% endhighlight %}

***************************************

1.`matplotlib.pyplot.fill(*args, **kwargs)`

[info:fill](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.fill)

2.`plt.grid(True)`

********************************

###3.多图叠加
[lines bars and markers example code: fill demo features.py](http://matplotlib.org/examples/lines_bars_and_markers/fill_demo_features.html#lines-bars-and-markers-example-code-fill-demo-features-py)


<figure>
    <a href="/images/1-figure-3.png"> <!--herf是超链接-->
        <img src="/images/1-figure-3.png"><!--img标签必须有src属性=“图片位置”-->
    </a>
</figure>


{% highlight python linenos %}
#coding:utf-8
"""
Demo of the fill function with a few features.

In addition to the basic fill plot, this demo shows a few optional features:

    * Multiple curves with a single command.
    * Setting the fill color.
    * Setting the opacity (alpha value).
"""
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 2 * np.pi, 100)#(0,2*pi),100等分
y1 = np.sin(x)
y2 = np.sin(3 * x)
plt.fill(x, y1, 'b', x, y2, 'r', alpha=0.3)#把两个曲线画在一张图上
#plt.grid(True)
plt.show()
{% endhighlight %}

**************************

###4.虚线和自定义虚线
[lines bars and markers example code: line demo dash control.py](http://matplotlib.org/examples/lines_bars_and_markers/line_demo_dash_control.html#lines-bars-and-markers-example-code-line-demo-dash-control-py)

<figure>
    <a href="/images/1-figure-4.png"> <!--herf是超链接-->
        <img src="/images/1-figure-4.png"><!--img标签必须有src属性=“图片位置”-->
    </a>
</figure>

{% highlight python linenos %}
#coding:utf-8
"""
Demo of a simple plot with a custom dashed line.

A Line object's ``set_dashes`` method allows you to specify dashes with
a series of on/off lengths (in points).
"""
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 10)#0-10等步长
line, = plt.plot(x, np.sin(x), '--', linewidth=2)

dashes = [10, 5, 100, 5] # 10 points on, 5 off, 100 on, 5 off 设置长横和短横
line.set_dashes(dashes)#按照dashes自定义虚线

plt.show()
{% endhighlight %}

*************************************
1.`line.set_dashes(dashes)`


2.`plt.plot(x, np.sin(x), '--', linewidth=2)`  
[info:.plot](http://matplotlib.org/1.3.1/api/pyplot_api.html#matplotlib.pyplot.plot)

*************************

**本文采用中国大陆版CC协议发布**
 
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)