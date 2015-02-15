---
layout: post
title: "Sublime Text插件MultiEditUtils介绍"
category: other
tags: [Sublime Text,插件,MultiEditUtils]
image:
  feature: article.jpg
comments: True
share: true
---

##MultiEditUtils

本文简单介绍一下Sublime Text 插件**MultiEditUtils**的功能和使用方法，图片均来自[官网](https://packagecontrol.io/packages/MultiEditUtils)


###1. 追加之前选择的内容(Extend the current selection with the last selection)

####`ctrl/cmd+alt+u`

![Alt text](/images/MultiEditUtils/Extend the current selection with the last selection.gif)

首先我们选中了`name`,然后我们又选择了`scope`，这时使用`ctrl/cmd+alt+u`,则可以同时选择两者。

然后我们配合sublime text本身的快捷键 `ctrl+t`可以实现两者交换。

###2. 分割选中内容(Split the selection.gif)

####`ctrl/cmd+alt+,`


![Alt text](/images/MultiEditUtils/Split the selection.gif)

首先选择内容，然后按下快捷键`ctrl/cmd+alt+,`，弹出输入框，让你选择以什么来分割。第2行的例子，我们输入`空格`，则光标被插入到每个空格前面。
如果我们不使用任何分割符，则如13行所示，每个字符都会被分割。

###3. 保持大小写(Preserve case while editing selection contents)

无快捷键，需要从 command palette (`ctrl+shift+p`)启动
![Alt text](/images/MultiEditUtils/Preserve case while editing selection contents.gif)

###4. 使光标位置一致，切换光标在选中目标的首末位置(Normalize and toggle region ends)

###` ctrl/cmd+alt+n`

![Alt text](/images/MultiEditUtils/Normalize and toggle region ends.gif)

有时候我们进行多重选择后，光标的位置并不一致，可能位于头部或者尾部，使用此功能可以将光标位置一致化，再次使用可以切换光标位置。

对于单一选中区域，也可以切换光标的位置
![Alt text](/images/MultiEditUtils/Normalize and toggle region ends-2.gif)


###5. 回到上次的位置(Jump to last region)

####`shift+esc`

![Alt text](/images/MultiEditUtils/Jump to last region.gif)

###6. 在多个选中区域巡回(Cycle through the regions)

####`ctrl/cmd+alt+c`
![Alt text](/images/MultiEditUtils/Cycle through the regions.gif)

在多个光标活动区域切换


###7.清理选中区域(Strip selection) 

####`ctrl/cmd+alt+s`

![Alt text](/images/MultiEditUtils/Strip selection.gif)

很多时候我们选中的区域里面包含了大量的空格，使用这个功能可以将它们去除。
对于演示图片，其中还包含了一个分割操作，以`;`为分隔符。

**因为国人的使用习惯，`ctrl+alt+s`，通常会被QQ占用，可以修改快捷键，不过比较好的方法还是从 command palette (`ctrl+shift+p`)启动**

###8. 移除选中的空白区域(Remove empty regions)

###` ctrl/cmd+alt+r`

![Alt text](/images/MultiEditUtils/Remove empty regions.gif)

###9. 快速选中全部内容--针对多重选择(Quick Find All for multiple selections)

###`ctrl+alt+f (Mac : cmd+alt+j`



![Alt text](/images/MultiEditUtils/Quick Find All for multiple selections.gif)


首先我们选中了`method`，然后选择了`obj`，使用`ctrl/cmd+alt+u`把前者追加，同时选择了两者。然后使用`ctrl+alt+f`选中了全部的`method`和`obj`

####记不住快捷键怎么办？

`ctrl+shift+p`，搜索MultiEditUtils可以出现全部的功能及其快捷键说明


-----------------------
**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)
