---
title: Git基础操作
date: 2022-05-29 22:32:30
tags: git
category: 后端
---

# Git简介 #

Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

官网地址为：https://git-scm.com/

**优点：**

适合分布式开发，强调个体；

公共服务器压力和数据量都不会太大；

速度快、灵活；

任意两个开发者之间可以很容易的解决冲突；

离线工作。

**缺点：**

代码保密性差，一旦开发者把整个库克隆下来就可以完全公开所有代码和版本信息；

权限控制不友好；如果需要对开发者限制各种权限的建议使用SVN。

![Git分布式控制总流程图](http://rco7kpoyg.hn-bkt.clouddn.com/flowchart.png)

# Git基础操作 #

### 克隆远程仓库内容

​	使用`git clone  `命令可以远程克隆代码托管平台，以GitHub平台为例，打开一个项目，在绿色的`code`处点击复制项目链接

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitcode.png)

在需要克隆的仓库点击鼠标右键打开`git bash`，在git bash中输入

```cpp
git clone https://github.com/crisp077/Git-test.git
```

<!--more-->

克隆成功后出现如下字样

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitclone.png)

打开克隆好的项目

![](http://rco7kpoyg.hn-bkt.clouddn.com/happen1.png)

我们可以发现一个GitHub项目就被克隆到了我们本地

### 拉取远程仓库内容

![](http://rco7kpoyg.hn-bkt.clouddn.com/change1.png)

​	在将项目克隆到本地文件后，如果项目更新，我们就可以利用`git pull `命令来同步远程仓库做出的改动

```cpp
git pull origin main
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitpull1.png)

就可以在仓库中看到更新的内容

![](http://rco7kpoyg.hn-bkt.clouddn.com/happen2.png)

​	**注意：**看现在仓库的主分支是`master`还是`main`，如果为master，则为`git pull origin master`

### 更新远程仓库内容

​	如果我们在本地项目上进行更新，那么就需要利用`git push`命令将本地项目的更新同步到远程仓库，在执行`git push`完整命令之前，我们可以利用`git status`命令来查看更新了哪些内容

```cpp
git status
```



![](http://rco7kpoyg.hn-bkt.clouddn.com/gitstatus.png)

由于我们之前在本地更新的内容属于工作区（会在status中显示为红色），在`git push`到远程仓库之前需要利用`git add`命令将其移动至暂存区（在status中显示为绿色），如果添加单个命令入缓存区可以输入`git add 文件名`，为了便利可以直接输入`git add .`，即添加所有文件

```cpp
git add .
```



![](http://rco7kpoyg.hn-bkt.clouddn.com/gitadd.png)

再次键入`git status`我们可以看到，本地新增的文件，其文件名由绿色变为红色，即从工作区成功移动到暂存区

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitstatus2.png)

在更新的文件存入暂存区后，还需要将其存入相对应的分支中，这是需要用的`git commit`命令，可以在commit后面键入对应的分支名，在分支名后可以添加本次更新内容的信息

```cpp
git commit -m "hellogit文件添加"
```

再次用`git status`查看状态，可以看到状态栏里面为空，说明上传成功

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitcommit.png)

​	**注意：**第一次安装Git后，commit可能会出现下面提示：

```cpp
git config --global user.email "you@example.com"
git config --global user.name "your name"
```

这时按照提示重新输入这两行命令，将自己邮箱和昵称输入即可

接下来就可以使用`git push`命令将更新内容上传至远程仓库

```cpp
git push origin main
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitpush.png)

我们打开GitHub可以看到更新的内容，以及后面的更新说明

![](http://rco7kpoyg.hn-bkt.clouddn.com/hellogitshow.png)

​	**注意：**在`git push`的过程中为了确保是本人对远程仓库的命令进行操作，一般会弹出弹窗让你输入远程仓库的密码，输入正确后就可以成功push；每次push都需要输入密码，显然很麻烦，我们可以通过`ssh-keygen -t rsa`来生成公钥，对远程仓库进行公钥配置，就可以实现免密推送。详情见[Github配置公钥](https://blog.csdn.net/redrose2100/article/details/121668161)。

### 版本追溯

​	在项目开发中经常会遇到各种问题，例如最新版本出现BUG，需要将版本回溯到以前的版本进行重新开发，这时候我们可以使用`git reset --hard 版本号` 命令来进行版本追溯，我们首先查看版本

![](http://rco7kpoyg.hn-bkt.clouddn.com/reset1.png)

然后进入

![](http://rco7kpoyg.hn-bkt.clouddn.com/reset2.png)

复制想要追溯的版本号，然后在本地项目中输入

```cpp
git reset --hard 2a871662d42a220a26f555a24cd0864a30352270
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/reset3.png)

就可以切换至要追溯的版本，想要切换至最新版本则可以键入`git reset --hard 最新版本号`。

### 分支管理

​	在一个项目开发中，通常会有正式发布版和开发版，正式发布版一般存放在`main/master`主分支而开发版本则存放在其他分支。对于一个`git clone`后的项目，我们可以通过`git branch`来查看该项目下有几个分支

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitbranch.png)

可以看到，这个项目中只有一个分支，`main`即主分支。我们可以利用`git branch 分支名`来创建一个其他的分支例如创建一个`develop`分支

```cpp
git branch develop
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitbranch2.png)

但是现在仍处于`main`分支，我们可以用`git checkout 分支名`切换到其他的分支

```cpp
git checkout develop
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitbranch3.png)

可以看出已经切换至`develop`分支，然后在`develop`分支下新建一个文件`develop.txt`

![](http://rco7kpoyg.hn-bkt.clouddn.com/gitbranch4.png)

然后用`git add`和`git commit`的一系列操作，将文件放入暂存区，准备更新远程仓库

```cpp
git add .
git commit -m "develop提交"
```

然后用`git checkout main`切换至主分支（也可以直接在`develop`分支直接输入，为了避免文件存放错误，一般在其他分支更新完成文件后建议切换至主分支进行操作），利用`git push`更新远程仓库（注意书写变化）

```cpp
git push origin develop:develop
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/develop.png)

点击`main`下面出现其他分支选项，点击切换至`develop`分支

![](http://rco7kpoyg.hn-bkt.clouddn.com/develop2.png)

可以看到`develop` 分支下存在`develop.txt`文件

![](http://rco7kpoyg.hn-bkt.clouddn.com/develop3.png)

当在`develop`或者其他分支的文件完全编写成功可以正式发布以后，我们就需要将`develop`或其他分支下的所做的更改应用到`main`分支上，这时候就需要用的`git merge`将`main`和`develop`进行合并，在`main`主分支中键入

```cpp
git merge develop
git push origin main
```

![](http://rco7kpoyg.hn-bkt.clouddn.com/develop4.png)

进入主分支`main`，看可以看到有`develop.txt`文件的存在

![](http://rco7kpoyg.hn-bkt.clouddn.com/develop5.png)

以上就是Git的基础的操作。
