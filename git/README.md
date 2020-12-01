# Learn Git



## Install

 - Windown系统可以在[Git官网](https://git-scm.com/downloads)下载、安装
 - Linux系统一般自带Git，手动安装
    ```shell
    apt install git
    ```

如果需要直接Push、Pull服务器上的代码，还需要先配置自己的`SSH key`：
```shell
# 生成ssh-key
ssh-keygen -t rsa -b 4096 -C "your_email" # your_email填自己的邮箱
```


如果选择的默认保存地址，在`～/.ssh/`可以找到一对公私钥。

```shell
cat ~/.ssh/id_rsa.pub
```
执行上面的命令之后，将得到的公钥添加到你的[GitHub账户设置](https://github.com/settings/keys)中。点击`New SSH key`，粘贴保存。



## Repository

Git是以仓库Repo为基础的。仓库可以理解为，一个需要版本控制的工程。

初始化一个仓库只需要在命令行执行：
```shell
git init
```



## Clone

当你需要将其他地方，比如GitHub上的仓库，复制到本地机器时。
```shell
git clone git@github.com:User/Repo.git
```



## Add & Commit

Git将文件系统区分为，工作区、暂存区和版本库三个部分。
 - `工作区`：就是你电脑中实际的看到的文件目录
 - `暂存区`：一般又被称为`索引`，存放在`.git`目录下，存放着工作区在上一个提交版本基础上的修改
 - `版本库`：存放在`.git`目录下，保存了历史提交的版本


修改的文件，需要先由工作区`加入(Add)`暂存区：
```shell
git add . # . 表示将所有修改加入暂存区
git add filename # 将一些修改加入暂存区，支持*等通配符
```


加入暂存区的文件，还需要再`提交(Commit)`之后，才会被保存到版本库中：
```shell
git commit -m "message" # message填写此次提交的说明
```


**注意：commit message是必须的**

commit message应该写的足够清晰，说明提交的目的或修改了的内容

建议使用动词开通的一句话写清楚，比如：
 - 5ba3db6 Fix failing CompositePropertySourceTests
 - 84564a0 Rework @PropertySource early parsing logic
 - e142fd1 Add tests for ImportSelector meta-data
 - 887815f Update docbook dependency and generate epub
 - ac8326d Polish mockito usageX



## Push

希望将本地的代码`推(Push)`到远端仓库，先要告诉他远端仓库在哪里。
```shell
git remote add origin git@github.com:User/Repo.git
```
当然如果是通过`clone`复制下来的代码，远端仓库的地址就默认就是源地址。


然后就可以`push`了。第一次的时候，需要加上`-u`的选项。
```
git push -u origin master # 第一次的时候加上 -u
git push origin master # 之后就不需要了
```



## Pull

当其他人在远端仓库提交了新的更新，你在做后续的修改之前，需要先把这些更新同步过来。
```shell
git pull
```
`pull`其实是git自动完成了`fetch`和`merge`两个步骤。如果没有冲突，git能很好的完成这个过程。如果有冲突，或者你想要的话，也可以自己手动依次完成这两个步骤。



## Branch

`branch`直接翻译过来叫`分支`，但你也可以把他理解成版本。就想在基础的开发之外，你有一个新的想法，但又担心新想法尝试失败了，会影响基础的开发。这时就可以建立一个新的分支，和基础的开发`并行`的推进，当测试新的分之能达到预期的想法的时候，再把它合并进来。


创建一个新的branch
```shell
git checkout -b <branch> # branch可以填你想要的分支名称
```



## Merge

大多数时候，Git都能自己完成`合并(merge)`。但有的时候可以两个提交都修改了，同一个文件的同一个地方，这时候Git就不知道该保存哪一个的修改了。

```shell
git merge <commit> # commit是你想要合并的另一个commit的id
```



## One More Thing

Git是一个非常完善，非常聪明的工具。使用的过程中，善用命令行中的提示信息，或者使用`-h`获得帮助。遇到问题不知道咋办时，不妨先`git status`看看情况再说。
