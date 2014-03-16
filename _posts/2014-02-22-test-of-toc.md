---
layout: post
title:	使用{:toc}自动生成目录或建立文内跳转
category: other
tags: [markdown, 目录生成]
image:
  feature: article.jpg
comments: true
share: true
---

目录

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

****************

**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)