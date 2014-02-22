---
layout: post
title:	使用{:toc}自动生成目录或建立文内跳转
category: other
tags: [markdown, 目录生成]
---


#目录
* Table of Contents
{:toc}




##1.自动生成目录

应用如下代码

	* Table of Contents
	{:toc} 
	这里需要一个空行
	##H1  

	###h1  

	####hh1

	##H2  

	###h2  

	####hh2


将得到如下图所示的目录
<figure>
    <a href="/images/toc-1.jpg"> <!--herf是超链接-->
        <img src="/images/toc-1.jpg"><!--img标签必须有src属性=“图片位置”-->
    </a>
</figure>


##2.建立文内跳转

不考虑链接美观的情况下，最简单的方法是使用html标签锚定一个位置，从而进行跳转。

{% highlight html %}
[文本内容](#jump)


<span id="jump">
</span>
{% endhighlight %}