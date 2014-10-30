---
layout: post
title: "使用jeklly及GitHub建立博客--安装配置篇"
category: trans
tags: [jeklly,博客]
image:
  feature: article.jpg
comments: True
share: true
---

在GitHub上托管我们的博客，可以使用[Jeklly](http://jekyllcn.com/)作为静态网页生成器。

###1.初始化GitHub Pages来建立一个blog
参考链接：https://pages.github.com/
初始化一个GitHub Pages非常容易，你可以直接点击上面的参考链接，也可以安装下面的步骤来进行

1. 创建一个仓库，仓库名为：`username.github.io`，其中`username` 是你的用户名。
2. 接下来，其实和初始化其他仓库没有什么区别，你可以使用命令行或者GitHub for Windows(Mac)来完成，下面针对命令行操作来说明
    -  克隆：`git clone https://github.com/username/username.github.io`
    - 在本地仓库中创建一个html：
    ```
    cd username.github.io
    echo "Hello World" > index.html
    ```
    - 更新远端仓库
    ```
    git add --all
    git commit -m "Initial commit"
    git push
    ```

    到此为止，我们已经创建了一个仓库，并且为其添加了一个html文件，等待10分钟左右，从浏览器打开`username.github.io` 即可查看你的blog，尽管此时非常简陋。
    PS:初始化的方式并不唯一，你可以像建立其他一般仓库一样建立你的blog仓库，只是它的名字比较讲究而已。

###2.安装Jeklly来处理Pages
其实一个最快的方案是不应该包含这一部分的，是否安装Jeklly其实并不影响你的blog，比如你可以直接clone一个开源的Jeklly的blog,但是在本机安装Jeklly是GitHub Pages鼓励的行为，你可以在本地调试你的网页，然后再推送，比较方便debug，也能有效减少推送次数。


参考链接：

- [使用Jekyll博客系统](https://help.github.com/articles/using-jekyll-with-pages/ )
- [Run Jekyll on Windows](http://jekyll-windows.juthilo.com/1-ruby-and-devkit/)
- [Jekyll 安装指南](http://jekyllcn.com/docs/installation/)

使用Linux和Mac OS的朋友可以直接看参考链接[Jekyll 安装指南](http://jekyllcn.com/docs/installation/)，这里我们主要针对Windows系统来讲解，使用的命令行工具是[git bash](http://msysgit.github.io/)


1. 安装Ruby
    - 下载[rubyinstaller](http://rubyinstaller.org/)并运行，安装结束后，在命令行工具中输入`ruby --version` 查看版本号，如果无法查看则可能需要重启或是手动添加bin目录到环境变量。

        ![Alt text](/images/jekyll/install-ruby-ready.png)

    - 下载[Ruby DevKit DevKit-mingw64](http://rubyinstaller.org/downloads/)有64位或32位可以选择，下载完成后打开，会询问解压位置（假定：C:\RubyDevKit），解压。解压后用`cd C:\RubyDevKit`定位到该目录下，然后运行`ruby dk.rb init`,初始化成功后会在目录下看到一个新文件`config.yml`，打开之后，添加本机ruby的安装路径，如图，然后运行`ruby dk.rb install` 即可完成。

        ![Alt text](/images/jekyll/install-rubykit-addpath.png)

2. 安装bundler
`gem install bundler`
3.  安装Jekyll
- `gem install jekyll`
**注意：出现`Gem::RemoteFetcher::FetchError)` 则可能需要代理服务器**
在本地仓库下使用`touch Gemfile`创建一个`Gemfile`,添加文本

        ```
        source 'https://rubygems.org'
        gem 'github-pages'
        ```
- 运行`bundle install`则安装所以必须的东西都会自动安装完成

4. 运行Jekyll
`bundle exec jekyll serve`
    ![Alt text](/images/jekyll/install-runjekyll.png)

    然后在浏览器访问：http://localhost:4000 即可查看本地仓库所构建的blog

    至此我们就配置好了Jekyll，我们可以进行一次commit来保存进度。不过，先不要着急，我们来看一下现在生成了哪些文件。

    运行`git status` 我们发现生成了如下文件，这些文件只需要留在本地即可，所以我们可以把他们添加到`.gitignore` 文件中。


