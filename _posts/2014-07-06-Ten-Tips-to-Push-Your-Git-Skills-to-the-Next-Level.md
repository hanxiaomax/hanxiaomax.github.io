---
layout: post
title: "让你Git水平更上一层楼的10个小贴士"
category: trans
tags: [git,技术翻译]
image:
  feature: article.jpg
comments: True
share: true
---

*本文翻译自[10 Tips to Push Your Git Skills to the Next Level--by Shaumik Daityari](http://www.sitepoint.com/10-tips-git-next-level/)*
*首发于[伯乐在线](http://blog.jobbole.com/75348/)*

**未经许可，禁止转载！**

-----------------------------

最近，我们发表了一些关于[Git基础知识](http://www.sitepoint.com/git-for-beginners/)和[在团队中使用Git](http://www.sitepoint.com/getting-started-git-team-environment/)的教程。我们之前讨论的那些命令，已经足够让帮助一个开发者在Git世界里生存了。本篇文章，我们将尝试探索如何更有效的安排您的时间以及如何充分使用Git提供的各种功能。

注意：本文中，一些命令包含含有方括号的部分(e.g. `git add -p [file_name]`).在这些例子中，您要在该处插入所需的数字，标示符等。而不需要保留方括号。

##1.Git自动补全

如果你在命令行中使用Git命令，每次手动输入命令是一件非常烦人的。为了解决这个问题，你可以很方便的开启自动补全功能。
在Unix系统下，运行以下指令来获取脚本：
{% highlight bash%}
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
{% endhighlight %}
然后，在您的`~/.bash_profile`文件中添加以下代码：
{% highlight bash%}
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
{% endhighlight %}
尽管我之前就提到过，在这里我仍要不厌其烦的说：如果你想使用Git提供的全部功能，你肯定是需要转而使用命令行来操作的。

##2.在Git中忽略文件
你是否对出现在你Git仓库中的已编译文件（比如.pyc）感到厌烦？
亦或是你已经对把它们加入Git中这件事感到忍无可忍了？
眼下就有一个可以让Git忽略特定文件或是目录的方法。只需要简单的创建一个`.gitignore`文件，然后列出你不想让Git跟踪的文件和目录即可。你可以使用感叹号(!)来指出例外的情况。
{% highlight bash%}
*.pyc
*.exe
my_db_config/
 
!main.pyc
{% endhighlight %}

##3.谁弄乱了我的代码？
出了问题后去责怪别人，是人类的天性。如果你的成品服务器出了问题，你可以非常轻松的把坏人揪出来——只需要使用`git blame`命令。
这个命令会显示文件中每一行的作者，最后一次改动后进行的提交以及该次提交的时间戳。。
`git blame [file_name]`
![Alt text](/images/git-10tips-01.png)

下图中，你可以看到在一个大型仓库中使用该命令是什么样子的。
![Alt text](/images/git-10tips-02.png)


##4.回顾仓库历史
在之前的教程中，我们了解了`git log`命令的用法，然而，它还有三个选项，你应该了解。
- `--oneline`——把每次提交间显示的信息压缩成缩减的hash值和提交信息，在一行显示。
- `--graph`——该选项会在输出界面的左手边用一种基于文本的图形表示法来显示历史。
如果你只是浏览一个单独分支的历史，那么这个功能是没有用的。
- `--all`——显示全部分支的历史

这里是以上命令综合使用的效果。

![Alt text](/images/git-10tips-03.png)

##5.绝不丢失一个提交信息
比方说，你提交了一个你不想要提交的代码，最后你通过使用硬重置(hard reset)使其回到了之前的状态。稍后，你意识到，在这个过程中你丢失了一些其他的信息，并想要退回或是至少能看一眼。`git reflog`命令可以帮你做到这一点。
一个简单的`git log`命令，显示你最近的提交信息，以及上一次，再上一次的提交信息，以此类推。
而`git reflog`显示的是所有head移动的信息。记住，它是在本地的，而不是你仓库的一部分，不会被包含在推送(push)和合并中(merge)。
如果我使用`git log`，我得到的提交信息是我的仓库的一部分。

![Alt text](/images/git-10tips-04.png)

然而`git reflog`显示了一个提交信息（`b1b0ee9` – `HEAD@{4}`），这是我使用硬重置(hard reset)时丢失的那个。

![Alt text](/images/git-10tips-05.png)

##6.暂存一个文件的部分改动
通常来讲，创建一个基于特性的提交是一个良好的做法，就是说，每次提交都必须代表一个新特性的产生或者是一个bug的修复。考虑一下，如果你修复了两个bug，或是添加了多个新特性但是却没有提交这些变化会怎样呢？在这种情况下，你可以把这些变化放在一次提交中。但是还有一个更好的方法：把文件分别暂存(Stage)然后分别提交。
比如说，你对一个文件进行了多次修改并且想把他们分别提交。这种情况下，你可以在添加命令(add)中加上`-p`选项
{% highlight bash%}
git add -p [file_name]
{% endhighlight %}
让我们演示一下。我在`file_name`文件中添加了3行文字，而且我只想提交第一行和第三行。我们先看一下`git diff`显示的结果。

![Alt text](/images/git-10tips-06.png)

然后，我们看一下，在添加命令(add)中加上`-p`选项后会发生什么。

![Alt text](/images/git-10tips-07.png)

看上去，Git假定所有的改变都是针对同一件事情的，因此它把这些都放在了一个块里。你有如下几个选项：
- 输入`y`来缓存该块
- 输入`n`不缓存该块
- 输入`e`来人工编辑该块
- 输入`d`来退出或进入下一个文件
- 输入`s`来分割这个块
对我们而言，我们肯定希望把它分成几个部分，有选择的添加一部分而忽略另一部分。

![Alt text](/images/git-10tips-08.png)

正如你所看到的，我们添加了第一行和第三行而忽略了第二行。你可以在之后查看仓库状态并进行提交。

![Alt text](/images/git-10tips-09.png)

##7.合并多次提交
当你提交你的代码进行审核并创建一个pull request时(在开源项目中常常发生这样的情况)，你经常会在代码被采纳前，要求修改一些代码。你进行了一些修改，而在下一次审核中，又会被要求进行另外的修改。你不知道还有多少次修改等着你，在你知道以前，你进行了多次额外的提交。理想的状态是，你可以使用`rebase`命令，把他们都合并成一次提交。
{% highlight bash%}
git rebase -i HEAD~[number_of_commits]
{% endhighlight %}
如果你希望合并最后两次提交，您需要以下命令
{% highlight bash%}
git rebase -i HEAD~2
{% endhighlight %}
使用该命令，你会进入一个交互式的界面，显示了最后两次提交，并且询问你要压缩哪些。理想状态是你`pick`最近的一次提交并把它和之前的提交`squash`。

![Alt text](/images/git-10tips-10.png)

接下来你会被要求为合并后的这次提交填写描述信息。这一个过程实际上重写了你的提交历史。

![Alt text](/images/git-10tips-11.png)

##8.保存尚未提交的改动
比方说你正在解决一个bug或是添加某个新功能，这时你突然被要求展示你的工作。你当前的工作还没有完成到进行提交的地步，而且你在这个阶段也没办法展示你的工作（如果不回退所有变化的话）。在这种情况下，`git stash`可以拯救你。stash命令本质上是保存了你全部的改动以供将来使用。保存你的改动，你只需要运行如下命令：
{% highlight bash%}
git stash
{% endhighlight %}
查看暂存列表，你可以运行如下命令：
{% highlight bash%}
git stash list
{% endhighlight %}

![Alt text](/images/git-10tips-12.png)

如果你不想保存了或是想要恢复这些改动，你使用如下命令：
{% highlight bash%}
git stash apply
{% endhighlight %}
在最后一张截图中，你可以看到，每一次保存都有一个标示符，一个独一无二的数字（尽管我们此处只有一次保存），万一你只想使用某些保存，你需要在`apply`命令后指明标示符。
{% highlight bash%}
git stash apply stash@{2}
{% endhighlight %}

![Alt text](/images/git-10tips-13.png)


##9.检查丢失的提交
尽管`reflog`是一种查看丢失提交的方法，但是它在大型仓库中行不通。这时就该`fsck`
(file system check)出场了。

{% highlight bash%}
git fsck --lost-found
{% endhighlight %}

![Alt text](/images/git-10tips-14.png)

这里你可以看到丢失的提交，你可以使用`git show [commit_hash]`来查看这些提交所包含的改动或者是使用`git merge [commit_hash]`来恢复它。
`git fsck`比`reglog`有一个优势。比如你删除了一个远端分支并且克隆了仓库，使用`fsck`命令你可以搜索并恢复该远端分支。

##10.cherry-pick命令
我把最优雅的Git命令留在了最后。`cherry-pick`是我最爱的Git命令，因为它的名字就意味着它的功能！
简而言之，`cherry-pick`是指从不同的分支里选择某次提交并且把它合并到当前的分支来。如果你在并行的开发某两个或多个分支，你可能会注意到有一个bug存在于所有的分支中。如果你在一个分支中解决了它，你可以使用*cherry-pick*来把这次提交合并进其他的分支而不会搞乱其他的文件或是提交。

让我们想象一个可以使用该命令的场景。我有两个分支，并且我想要把`b20fd14: Cleaned junk`这次提交使用*cherry-pick*的方法放入到另一个分支。

![Alt text](/images/git-10tips-15.png)

我切换到我想要放入该提交的分支，然后运行如下命令：
`git cherry-pick [commit_hash]`

![Alt text](/images/git-10tips-16.png)

尽管我们本次使用`cherry-pick`没什么问题，但是你应该清楚这个命令会带来冲突，请谨慎使用。


##小结

说着说着我们就来到了所有技巧的末尾，我认为这些技巧会让你的Git水平更上一层楼。
Git是最棒的，只要你能想得到，它就能做得到。
因此，要经常挑战自己的Git水平。最后，你很有可能会学到新的东西。

