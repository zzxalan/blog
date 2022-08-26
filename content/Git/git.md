---
title: "git使用手册"
description:
toc: true
authors:
- 小z
tags:
categories:
series:
date: 2022-07-28T15:11:43+08:00
draft: false
---

### 基本概念

#### 工作区（Working Directory）

就是你在电脑里能看到的目录

#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

<!--more-->
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。


![git-repo](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

### 自报家门

~~~ git
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
~~~

--global 对所有的仓库生效，不添加则只对当前仓库生效

### 基本命令

~~~ git
git init                        #创建仓库
git add **.txt                  #文件添加到暂存区
git rm **.txt                   #删除暂存区中的文件
git commit -m "提交说明"         #提交修改(暂存区文件提交到分支中)
git log [--pretty=oneline]      #查看提交历史，--pretty=oneline 简洁模式
git status                      #查看当前状态
~~~

### 版本管理

~~~ git
git checkout -- **.txt     #把文件从工作区的修改全部撤销（回到暂存区后的状态）(删除也是一种修改)
git reset HEAD  **.txt     #把暂存区的修改撤销掉（unstage），重新放回工作区

git reset --hard [版本号 | HEAD^ | HEAD^^ | HEAD~100]  #版本回退到指定版本、上一个版本、上上个版本、往上100个版本
git reflog  #输入命令历史
~~~

### 远程仓库

~~~ git
git remote add origin git@github.com:zzxo/learngit.git  #关联远程仓库，origin是给远程仓库命名(origin默认习惯命名)
git push -u origin master                               #本地分支推送到远程仓库，-u 关联分支
git remote -v          #远程仓库信息
git remote rm origin   #解除与远程仓库的绑定关系(不会删除远程仓库)
git clone git@github.com:michaelliao/gitskills.git  #从远程仓库克隆一个本地仓库

git pull     #抓取远程的新提交
git rebase   #rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了
~~~

### 分支管理

~~~git
git branch [-a]      #查看分支 带*表示当前分支   -a 查看所有分支包括远端
git branch dev       #创建分支
git branch -d dev    #删除分支
git checkout -b dev  #创建并切换分支(相当于git branch + git checkout) 不推荐分支操作用checkout
git switch -c dev    #切换分支, -c 当不存在该分支时创建
git merge dev        #合并指定分支到当前分支(合并dev到当前分支)
~~~

#### 分支管理策略

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

### 工作区暂存

~~~ git
git stash         #暂存工作区
git stash list    #暂存列表
git stash apply   #恢复暂存内容
git stash drop    #删除暂存内容
git stash pop     #恢复并删除暂存内容
git stash apply stash@{0}  #恢复指定暂存内容

git cherry-pick 4c805e2  #复制某个分支提交内容到当前分支，省掉手动操作
~~~

### 子模块

.gitmodules 子模块配置文件

~~~git
git submodule add https://github.com/chaconinc/DbConnector   #添加子模块(.gitmodules文件会增加一条记录)
git submodule init   #初始化本地配置文件
git submodule update  #从该项目中抓取所有数据并检出父项目中列出的合适的提交
git submodule update --init --recursive  #合并操作
git clone --recurse-submodules git://****.git   #--recurse-submodules会初始化并更新子模块
~~~


### 标签

- 命令`git tag <tagname> [commit id]`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

- 命令`git tag`可以查看所有标签



- 命令`git push origin <tagname>`可以推送一个本地标签；

- 命令`git push origin --tags`可以推送全部未推送过的本地标签；

- 命令`git tag -d <tagname>`可以删除一个本地标签；

- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签

