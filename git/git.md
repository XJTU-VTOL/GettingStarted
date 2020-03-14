# Git 基础

[查看git的官方文档](https://git-scm.com/book/zh/v2)

## Git基础

## 01 | 课程综述

发展历史

- 无版本控制
- 集中式VCS
  - 版本控制
  - 分支管理
  - 需要连接服务器
- 分布式VCS



## 02 | 安装Git

[下载git](https://git-scm.com/downloads)

[git中文参考](https://git-scm.com/book/zh/v2)



## 03 | 使用Git之前需要做的最小配置

- 配置`user.name`和`user.email`：

    ```shell
    git config --global user.name 'your_name'
    git config --global user.email 'your_email'
    ```

- config的作用域：

    ```shell
    git config --local # 对某个仓库有效
    git config --global # 对当前用户所有仓库有效
    git config --system # 对系统所有登录用户有效
    ```

- 显示config的配置，加`--list`：

    ```shell
    git config --list --local
    git config --list --global
    git config --list --system
    ```

- 清除设置

  ```shell
  git config --unset --local user.name
  git config --unset --global user.name
  git config --unset --system user.name
  ```




## 04 | 创建第一个仓库并配置local用户信息

- 把已有项目代码纳入Git管理：

  ```shell
  git init
  ```

- 新建的项目直接用Git管理：

  ```shell
  git init your_project
  ```



## 05 | 通过几次commit来认识工作区和暂存区

- 将工作空间的变更提交到暂存区：

  ```shell
  git add . # 包括文件的修改和增加，不包括删除文件
  ```

  ```shell
  git add -u # 包括文件的修改和删除，不包括没有纳入git管理的新文件
  ```

- 提交暂存区更新

  ```shell
  git commit -m 'message'
  ```



## 06 | 给文件重命名的简便方法

```shell
git mv old_name new_name
```



## 07 | 通过git log查看版本演变历史

```shell
git log --all # 查看所有分支的历史
git log --all --graph # 查看图形化的 log 地址
git log --oneline # 查看单行的简洁历史。
git log --oneline -n4 # 查看最近的四条简洁历史。
git log --oneline --all -n4 --graph # 查看所有分支最近 4 条单行的图形化历史。
git help --web log # 跳转到git log 的帮助文档网页 
```



## 08 | gitk：通过图形界面工具来查看版本历史

`gitk`可通过图形化界面查看版本历史，可在命令后加文件名查看单个文件的修改历史。



## 09 | 探密.git目录

`.git`路径下的主要文件：

- `HEAD`：指向当前的工作分支
- `config`：存放本地仓库（local）相关的配置信息。
- `refs/heads`：存放分支
- `refs/tags`：存放`tag`，又叫里程牌 （针对有重要意义的commit）
- `objects`：存放对象 ，`.git/objects/ `中对象以对象的40位哈希值来表示，前2位做子文件夹名，后38位做文件名。



`git cat-file`：显示版本库对象的内容、类型及大小信息。

- git cat-file -t b44dd71d62a5a8ed3 显示版本库对象的类型
- git cat-file -s b44dd71d62a5a8ed3 显示版本库对象的大小
- git cat-file -p b44dd71d62a5a8ed3 显示版本库对象的内容



## 10 | commit、tree和blob三个对象之间的关系

`commit`：包括一次`commit`提交的内容和描述信息，提交的内容用一棵`tree`表示

`tree`：包括一个文件夹中的文件和子文件夹，文件用`blob`表示，子文件夹用`tree`表示

`blob`：包括一个文件对象，内容相同的不同文件用同一个`blob`对象表示



## 11 | 小练习：数一数tree的个数

`tree`数量 = `commit`数量 + 非空文件夹数量

**注意**：Git不会将空文件夹加入版本控制中



## 12 | 分离头指针情况下的注意事项

分离头指针：`HEAD`只指向一个`commit`而没有与一个分支挂钩。此时产生的`commit`在切换到其他分支之后会被Git清理掉。

分离头指针适合用于临时的测试，如对修改不满意，切换回其他分支即可，不需要创建删除分支。



## 13 | 进一步理解HEAD和branch

- `HEAD`直接或者通过`branch`间接指向`commit`
- `HEAD^`和`HEAD~`都可以表示头节点的父亲
- [`^` 和`~`的区别](https://stackoverflow.com/questions/2221658/whats-the-difference-between-head-and-head-in-git)：
  - Use `~` most of the time — to go back a number of generations, usually what you want
  - Use `^` on merge commits — because they have two or more (immediate) parents



# 独自使用Git

## 14 | 怎么删除不需要的分支？

删除分支：

```shell
git branch -d branch_name 
git branch -D branch_name 
```

- 使用`-d`删除之前Git会判断该分支上的开发是否被`merge`到其他分支，如果没有则不能删除。

- 使用`-D`可以强制删除分支



## 15 | 怎么修改最新commit的message？

```shell
git commit --amend 
```

修改最近一次`commit`，`message`和内容都可以修改



## 16 | 怎么修改老旧commit的message？

```shell
git rebase -i base 
```

- `rebase`是将`HEAD`指向`base`之后，使用新的`message`和内容重新提交代替原来的`commit`
- 这里的base是需要变基的`commit`的父节点
- 修改第一次`commit`，父节点使用`--root`



## 17 | 怎样把连续的多个commit整理成1个？

```shell
git rebase -i base 
```



## 18 | 怎样把间隔的几个commit整理成1个？

```shell
git rebase -i base 
```



## 19 | 怎么比较暂存区和HEAD所含文件的差异？

```shell
git diff --cached
git diff --staged
```



## 20 | 怎么比较工作区和暂存区所含文件的差异？

```shell
git diff # 工作区 <--> 暂存区
git diff HEAD # 工作区 <--> HEAD
git diff --cached # 暂存区 <--> HEAD
```



## 21 | 如何让暂存区恢复成和HEAD的一样？

```shell
git reset HEAD
```

 

## 22 | 如何让工作区的文件恢复为和暂存区一样？

```shell
git checkout
```

- `git checkout`针对工作区
- `git reset`针对暂存区



## 23 | 怎样取消暂存区部分文件的更改

```sh
# git reset HEAD <file>
git reset HEAD -- README.md
```



## 24 | 消除最近的几次提交

```sh
git reset --hard <commit>
```

**注意：使用此操作之后HEAD会退回到指定commit，且工作区和暂存区的更新都会丢失。**



## 25 | 看看不同提交的指定文件的差异

```sh
# git diff [options] <commit> <commit> [--] [<path>...]
```



## 26 | 正确删除文件的方法

```sh
# git rm [<options>] [--] <file>...
git rm README.md
```

删除文件同时提交暂存区。



## 27 | 开发中临时加塞了紧急任务怎么处理？

主要针对当前分支的新工作还不想提交，但需要在其他分支紧急修复bug的情况。

```sh
git stash # 把暂存区和工作区的改动保存起来
git stash list # 显示保存列表
git stash pop # 恢复进度到工作区，并删除stash堆中储存
git stash apply # 恢复进度到工作区，不删除stash堆中储存
```



## 28 | 如何指定不需要Git管理的文件？

使用**.gitignore**文件。

- `doc` ：忽略文件夹和文件
- `doc/` ：忽略文件夹，不忽略文件



## 29 | 如何将Git仓库备份到本地？

- 通过`git push`备份到其他`remote`仓库
- 本地使用智能协议`file:///path/repo.git`相比哑协议传输可见，速度更快
- 使用`git clone --bare`克隆不带工作区的裸仓库



# Git与GitHub同步

## 30 | 注册一个GitHub账号

[github.com](https://github.com)



## 31 | 配置公私钥

[Connecting to GitHub with SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

- 一对公私钥可以使用在多个服务器上，所以先确认`~/.ssh`路径下是否已有`ssh key`

- 若没有公私钥，创建一对公私钥：

  ```shell
  ssh-keygen -t rsa -b 4096 -C "your_email"
  ```

- 在`~/.ssh`路径下可以找到新建的：
  - 公钥：`id_rsa.pub`
  - 私钥：`id_rsa`
- 将公钥[添加到GitHub](https://github.com/settings/keys)，点击`New`并将公钥复制进去保存即可



## 32 | 在GitHub上创建个人仓库

[新建仓库](https://github.com/new)



## 33 | 把本地仓库同步到GitHub

- 添加远端仓库：

  ```shell
  git remote add <repo_name> <url>
  ```

- 对于不是`fast forward`的远端仓库，需要先`fetch`再`rebase`或者`merge`

  ```shell
  git fetch <repo>
  git merge --allow-unrelated-histories <repo>/<branch>
  ```

- 推送到远端仓库：

  ```shell
  git push <repo>
  ```



# Git多人单分支集成协作

## 34 | 不同人修改了不同文件如何处理？

先把其他人在不同文件的更新`fetch`下来之后就可以`merge`到一起



## 35 | 不同人修改了同文件的不同区域如何处理？

修改文件的不同区域Git也可以完成`merge`



## 36 | 不同人修改了同文件的同一区域如何处理？
- 执行`merge`之后Git自动处理不同文件或者同文件不同区域的修改

- 手动处理同一区域的冲突：

  - 命令行打开每个冲突的文件，手动处理

  - 图形化界面处理

    ```shell
    git mergetool
    ```

- 处理完冲突之后提交`commit`



btw，可以研究一下Git是如何判断冲突是否属于同一区域，是否可以自动处理？可以了解一下Git执行merge时的策略有哪些？



## 37 | 同时变更了文件名和文件内容如何处理？

我修改其他人变更了文件名的文件的情况Git可以自动处理



## 38 | 把同一文件改成了不同的文件名如何处理？

不同人修改同一文件成了不同的文件名时，Git不会处理，会保留所有的文件



# Git集成使用禁忌

## 39 | 禁止向集成分支执行push -f操作

禁止强制`push`



## 40 | 禁止向集成分支执行变更历史的操作

禁止对集成分支做`rebase`,不能更改以后的历史



# 初识GitHub

##  41 | GitHub为什么会火？



## 42 | GitHub都有哪些核心功能？

- Code review
- Project management
- Integrations
- Team management
- Social coding
- Docmentation
- Code hosting



## 43 | 怎么快速淘到感兴趣的开源项目?

- Github默认搜索项目的**名称**和**描述**
- 善于使用Github的`Advanced search`
  - `in:readme`：搜索readme
  - `stars>1000`：星的数量



## 44 | 怎样在GitHub上搭建个人博客

https://github.com/barryclark/jekyll-now



## 45 | 开源项目怎么保证代码质量？



## 46 | 为何需要组织类型的仓库？



# GitHub团队协作

## 47 | 创建团队的项目



## 48 | 怎样选择适合自己团队的工作流？



## 49 | 如何挑选合适的分支集成策略？

- Creat a merge commit
- Squash and merge
- Rebase and merge



## 50 | 启用issue跟踪需求和任务



## 51 | 如何用project管理issue？


## 52 | 项目内部怎么实施code review？



## 53 | 团队协作时如何做多分支的集成？



## 54 | 怎样保证集成的质量？



## 55 | 怎样把产品包发布到GitHub上？



## 56 | 怎么给项目增加详细的指导文档？



# GitLab实践

## 57 | 国内互联网企业为什么喜欢GitLab？



## 58 | GitLab有哪些核心的功能？



## 59 | GitLab上怎么做项目管理？



## 60 | GitLab上怎么做code review？



##  61 | GitLab上怎么保证集成的质量？



## 62 | 怎么把应用部署到AWS上？

