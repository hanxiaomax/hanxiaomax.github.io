---
layout: post
title: "Makefile 101 for dummies just like me"
category: cpp
tags: [python,matplotlib]
modified: 2014-02-21
image:
  feature: 2000px-Matplotlib_logo.svg.png
comments: true
share: true
---

[lines_bars_and_markers example code: barh_demo.py](http://matplotlib.org/examples/lines_bars_and_markers/barh_demo.html)


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

**本文采用中国大陆版CC协议发布**
 
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/1807732335/AvK7VrQlp?type=like)