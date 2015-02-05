---
layout: post
title: "Learn Git in One hour"
category: trans
tags: [git,Learn X in One hour]
image:
  feature: article.jpg
comments: True
share: true
---

>本系列文章，受到[Learn X in Y minutes](http://learnxinyminutes.com/)以及[知乎-哪些技能，经较短时间的学习，就可以给人的生活带来巨大帮助？](http://www.zhihu.com/question/20846401)等系列教程、文章的启发。
>
本系列可以看做是`入门教程`，`cheatsheet` 或是`笔记心得`，其重点在于利用尽**可能直白**的语言，介绍某项技术最**核心的部分**，并**显著改善**我们的生活和工作状态，并为后续的学习打下基础。

本文为[Learn Git in One hour]()系列文章第一篇，重点在[Git](http://baike.baidu.com/link?url=-YXe9s9pkhSJ1Q3Byj_m0KmX7_UwzQ6ijo2mjV7e-fMi0LrNAxuaXm9qwxXXiR42RptrL_QZ_knbworGwlQSuJubUJz8K6EUSmTcJqdyUse)入门，希望读完本文后，您就可以开始使用Git进行工作了。

------------------
##零、Git哲学
####0.哲学
####1.版本号(commit-id) :
一个SHA1计算出来的一个非常大的数字，用十六进制表示,代表每一次`commit`，一般使用前6位即可。
![Alt text](/images/learngit/log.png)

####2.HEAD指针:
表示当前版本，HEAD~{n}表示当前版本之前的第n个版本

####3.工作区（Working Directory）和 版本库（Repository）
![Alt text](/images/learngit/1.png)
版本库类似于我们玩游戏的存档区，每一次提交到版本库，就有一个存档记录(save)，方便日后进行回退(load),而存档的名字，就是版本号(commit-id)

####4.暂存区(stage) 和 分支(branch)
![Alt text](/images/learngit/2.png)

通过`add`放入暂存区，在通过`commit`提交到当前分支，只有被放入暂存区的文件，我们可以多次`add`然后统一`commit`，提交到版本库。

**未追踪状态（Untracked）**：从未被放入过暂存区


##一、最简流程及常用命令
- `git init` : 初始化
首先需要把一个普通文件夹初始化为一个git仓库

- `git add <file> <file_1>....`:加入缓存区

- `git commit -m "commit message"`:提交缓存区内容到分支

- `git status` : 查看状态
查看当前版本库的状态

- `git diff` : 对比变化
查看文件改动前后的变化

- `git log`:查看提交记录  
  `git log --pretty=oneline`  
  `git reflog` :查看命令历史  

-  `git push origin master` : 推送到远端仓库origin/master


![Alt text](/images/learngit/2.png)
大部分时候，我们只是在循环使用这几个命令。
其中我们经常会使用`git status`来查看当前状态：

![Alt text](/images/learngit/untracked.png)：

**新创建了`test.md`后，会提示找到未追踪的文件。**

![Alt text](/images/learngit/not_staged.png)：

**修改后，尚未添加到暂存区（commit不会将其提交）**

![Alt text](/images/learngit/w.png)：

**添加到了暂存区（commit会将其提交）**

![Alt text](/images/learngit/aftercommit.png)：

**运行`commit`之后，暂存区被提交。工作区干净（git没有发现工作区的任何改动）**





##二、回退与删除
>回退大法好

我们经常会需要对版本进行回退，这种回退可能发生在各个阶段，git都能够很好很方便的操作，这也是git强大的地方。

###0.查看版本
回退首先要查看**版本**,不然我们没办法确定要回退到什么位置。此处会用到`HEAD`和`版本号`两个东西。以及`git log`命令

使用`git log`或者`git log --oneline`可以查看我们之前的commit记录，也就是我们之前的存档。

![Alt text](/images/learngit/log.png)




###1.尚未暂存(before `add`)
`git checkout -- <file>`：丢弃**工作区**修改。注意是工作区，对暂存区和版本库用此命令是无法修改的

`git reset HEAD <file>`:  丢弃**暂存区**的修改（unstage）

###2.尚未提交(before `commit`)
- 暂存区有修改可以提交!

- 撤销当前暂存区的修改

- 暂存区全部修改被撤销，工作区有未暂存的文件!

###3.已经提交(after `commit` before `push`)
回退版本，回退后，工作区和暂存区都是干净的。（--hard一定，其他不一定）
`git reset <commit-id>`,Git会把HEAD指针，指向我们回退的那个版本。

![Alt text](/images/learngit/head.png)



###4.已经推送(after `push`)
首先把本地回退，然后`git push -f origin master`进行强行推送，如果不使用`-f`,则出现non-fast-forward错误。


###5.删除文件
在工作区，删除文件后，用`git status` 会检测到有文件被删除（工作区），如果我们确实要删除，则使用`git rm`来删除（同时会进行暂存，类似于add），然后，`git status`会检测到有文件被删除（暂存区），此时我们已经可以提交了。
当然，`删除`也是一种修改，我们可以使用**回退大法**来撤销我们的修改。


所有被提交到版本库的文件，都不需要担心误删，我们可以通过回退来找回文件，但是回退是有[代价]()的


##三、远程仓库（coding socially）
###1. SSH key
`$ ssh-keygen -t rsa -C "youremail@example.com"`
将会在用户主目录里生成`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，如果你已经有了这两个文件，就不需要上述操作了。
绑定SSH key到GitHub

###2. 创建并添加远程仓库
`git remote add origin git@github.com:hanxiaomax/learngit.git`
远程库的名字默认是origin



###3. 推送
`git push -u origin master`
`-u`参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
` git push origin master`

###4. 克隆
`git clone git@github.com:hanxiaomax/gitskills.git`

###5. 多种协议
`https`和`ssh`

`https://github.com/hanxiaomax/hanxiaomax.github.io.git`
如果添加的仓库地址是这样的，则在`push`时需要输入密码，因为是`https`方式。
`ssh`方式更为方便，速度也比较快。

##四、分支管理

###1. 简介
分支是git系统非常重要的一部分，我们可以认为Git世界并非仅有一条时间线，我们可以在不同的时间线上开发，然后合并。当然，这之间不能有冲突。。

###2. 创建分支
`git checkout -b dev`创建并切换分支
相当于
`git branch dev 创建分支`
`git checkout dev 切换分支`

###3. 查看分支
`git branch`
当前分支前面会有`*`

###4. 合并分支
合并B分支到A首先需要合并切换到

###5. 删除分支
`git branch -d <name>`

###6. 分支冲突
假设我们从master创建了一个分支dev，然后进行了一些修改，同时，master分支也进行了一些修改，很显然，在master和dev直接，可能对同一个部分进行了不同的修改，这样在我们合并他们的时候，就会产生冲突。
`git merge dev` 会报告有冲突，那么我们使用`git status` 来查看具体冲突  
Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们需要人工的对冲突处进行确认和取舍，选择两者之一或者进行其他的修改。修改此处为我们需要的状态。再次`add`,`commit`

###7. 分支树
`git log --graph --pretty=oneline --abbrev-commit`

###8. fast-forward方式
fast-forward方式是指在合并时，master的指针直接指向当前分支。当可以使用fast-forward方式时，git会尽量使用它。
`git merge dev`
但是这种情况下，如果删除分支，则会丢失分支信息，我们可以强制关闭fast-forward方式：
`git merge --no-ff -m "no-ff" dev`
这样git会在合并时，创建一个commit，所以自然我们需要写commit massage

###9. 良好的分支习惯
通常我们不希望在`master`上面直接开发，而是把`master`当做**成品仓库**，只有当准备发布新版本时才会去向`master`合并。
因此，我们首先会创建一个`dev`分支，然后大家从这个分支把代码克隆到自己的机器上进行开发，并时常的进行提交和推送，在`dev`分支上进行合并。然后在某个大版本完成准备发布的时候，把`dev`合并到`master`即可。

###10. *紧急bug分支（在工作区未提交时创建分支）*
请查看：
`git stash` 暂存
`git stash list` 查看
`git stash apply` 恢复
`git stash drop` 删除

- [让你Git水平更上一层楼的10个小贴士](http://hanxiaomax.github.io/trans/Ten-Tips-to-Push-Your-Git-Skills-to-the-Next-Level/)中的第8条。
- [廖雪峰的官方网站-->Git教程-->Bug分支](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000)
- [learnGitBranching](http://pcottle.github.io/learnGitBranching/)---在线学习Git分支

###11. *强制删除分支*
`git branch -D <branch-name>`

###12. 推送分支
`git remote`查看远程仓库
`git push origin master` 推送本地master分支到远程仓库origin的master分支
`git push origin dev` 推送本地dev分支到远程仓库origin的dev分支。

###13. 抓取分支
`git clone git@github.com:hanxiaomax/learngit.git`
如果这样clone的话，在本地只能`master`分支

如果需要其他分支：
`git checkout -b dev origin/dev`
这样，会在本地创建`dev`分支，并且把它和远端`origin`分支的`dev`对应起来。同时，切换到dev分支。
当我们向远端分支提交时，可能会遇到non-fast-forward，这种情况有几种处理方式：

1)：**强行上传**:·
`git push -f origin master`，这种方式一般不用，因为会把远端的更新的文件，替换为本地较旧的文件，不过如果你像远端推送了不希望推送的内容，可以用这种方法删除。
2)：**解决冲突**
`git pull`可以先把远端仓库最新的代码拉到本地，此时会自动向本地分支合并，此时会遇到冲突。解决冲突，然后再次提交，上传。  

##五、GitHub

假设有项目仓库A，你想要参与：
1. `fork`仓库A。因为你没有权限向A直接推送，它是属于仓库原主人的（你的ssh key不在它的列表中）。
2. `clone`仓库A。下面你需要clone fork后的仓库yours/A到本地，进行开发
3. `push`。把你写的代码先push到你的yours/A仓库中。
4. `pull request`向原仓库A的主人申请，让他从远端把你yours/A仓库中代码pull到远端仓库A。至此，就完成了贡献代码。



##六、忽略文件
创建`.gitignore`文件，可以让git不去追踪某些不必要的文件。

`/`：目录；   
`*`：通配符  
`?`：通配符  
`[]`：单字符匹配列表  
`!`：不忽略  


##七、参考文件及扩展阅读:

- [廖雪峰的官方网站-->Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [learnGitBranching](http://pcottle.github.io/learnGitBranching/)
- [让你Git水平更上一层楼的10个小贴士](http://hanxiaomax.github.io/trans/Ten-Tips-to-Push-Your-Git-Skills-to-the-Next-Level/)
- [Git使用教程 by 龙恩0707 ](http://www.cnblogs.com/tugenhua0707/p/4050072.html)
- [Pro Git 中文版](http://git-scm.com/book/zh/v1)
- **帮助文档：**`git <命令> --help`


----------------------
**本文采用中国大陆版CC协议发布**  
作者保留以下权利：  
1. 署名（Attribution）：必须提到原作者。  
2. 非商业用途（Noncommercial）：不得用于盈利性目的。  
3. 禁止演绎（No Derivative Works）：不得修改原作品, 不得再创作。   
新浪微博 [@XX含笑饮砒霜XX](http://weibo.com/smilingly1989)
