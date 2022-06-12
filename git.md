## 基础

删除暂存区的所有文件 rm .git/index

```bash
git help --web [如 log]
```

**commit(对象) **:

*PS:以下的内容可以使用 git cat-file -p 哈希值来查看*

- tree: 这一次的提交,仓库中的文件的关系(包含目录和文件)

  	- tree:指文件夹
  	- blob:指文件本身,其值与文件的名称无关,只与文件的内容有关.内容相同就是同一个blob
- parent: 上一次提交的哈希值
- author: zhangeaky
- committer:

---

在实验室和寝室分别工作和同步:

```bash
1.zai从github上将在实验室创建的仓库clone下来
git clone [as yours]
2.在这个clone下来的仓库中进行工作,并保存提交等处理后
git push origin master(可以视情况而定)
3.此时github上的仓库已经更新,在实验室使用将更新pull下来
git pull [remote名字] [分支]
4.然后就可以在实验室使用在寝室完成的commit的基础上继续工作
5.再次回到寝室后可以在原来的仓库中使用git pull 命令进行更新仓库内容
```



---

### 添加最小配置:

```bash
git config --local user.name 'zhangEaky'
git config --local user.email 'zyk091004@163.com@163.com'
```

- 参数区别:

```bash
git config --local ##只对某个仓库有效,切换到另外一个仓库失效
git config --global ##当前用户的所有仓库有效,工作当中最常用
git config --system ##系统的所有用户,几乎不用
```

- 查看配置:

```bash
git config --list --local ##只能在仓库里面起作用, 普通路径git不管理
git config --list --global
git config --list --system
```

- local配置信息存储在.git/config里面
- global配置信息在个人home目录下的.gitconfig里面
- system应该在git安装目录的下

### 文件重命名操作

1. 使用原生方法进行文件重命名

```bash
## 用shell内建立命令进行重命名产生的后果是:原来文件被删除,同时产生一个未被跟踪的新文件
git add [renamed file] #将重命名的文件加入到暂存区
git rm  [former file] # 将之前的文件删除
```

2. 使用git方法

```bash
git mv readme readme.md  #将文件read改成readme.md
```

### 正确删除文件的方法

```bash
git rm [文件名] 
```

### 提交

>  查看某一次提交的内容

```bash
git commit -m ‘描述’ #提交某一次更改至版本库
git log #[可以查看到这次提交的哈希值]
git show [哈希值]
```

>  一次commit包含哪些内容?

1. 工作目录的快照名称

2. 一条你写的注释

3. 提交者信息

4. 父提交的哈希值

 

 ### 什么是**分支**?

> ​	分支是独立的开发空间,建立的不同的分支在其中工作的时候互相不影响,在不同分支的同名文件上修改不会产生连带影响..当需要集成的时候,两个不同的分支可以集成到一个公共的分支上面.
>
> 例如,当前在master分支上创建一个temp分支,temp分支就会继承master分支上已有的提交

```bash
git branch -v  #查看分支

$ git checkout -b temp 	#创建新分支,默认是在原来的分支的基础上

$ git checkout -b temp  [某一次commit的哈希值] #创建新分支

$ git checkout [分支名] #切换到某一个分支

$ git branch -av #查看当前的分支

$ git branch -d [分支名] #删除不想要的分支

$ git branch -D [分支名] #如果上述命令报错 
```

### 暂存区

- 暂存区

> 查看暂存区文件信息

```bash
$ git ls-files

$ git add files(可以添加多个文件) #添加到暂存区 

$ git add -u  更新已跟踪的文件

$ git add --all(-A)

$ git reset [文件名] #可以取消暂存

# 删除暂存区的文件
$ git rm --cached filename
```

- 将暂存区和工作区的**变更**清除

```git
例如,您将一个文件的名字使用git的方式进行了更改,或者给您的C++工程文件中加了一段不是很好的算法,想要恢复到算法添加之前,都可以使用这条命令,但请记住这种方法是危险的,请在使用前三思而行.

命令本身将不会影响项目的版本和提交历史
```

```bash
$ git reset --hard
```

- 暂存区与HEAD的比较

```bash
git diff --cached 
```

- 暂存区与工作区的比较

```bash
git diff (即默认的情况下的比较)

git diff [文件名]  只查看指定文件之间的差别


```

- 移动了暂存区文件的路径

- 暂存区与恢复成和HEAD一样

$ git reset HEAD  (改暂存区的内容)

工作区文件恢复成和暂存区一样

$ git  checkout  [文件名]	(改工作区的内容)

暂存区部分文件的更改

$ git  reset HEAD -- [想恢复的文件名]	(改暂存区的内容)

删除暂存区的文件

$ git  rm --cached (文件名,而且是和暂存区路径相同的,不是改动后的文件路径)

撤销几次commit

 **暂存区和工作去的数据也会变到那一次的commit之前,这一条命令要慎用.**

```bash
git reset --hard [想要变成的commit的哈希值]
```

查看不同commit下文件的差异

$ git diff master temp --index.[指定文件名]	(比较两个分之间)

$ git diff  [commit1哈希值] [commit2哈希值] --指定文件



### git log 版本

```bash
$ git log --online  #简洁的显示变更历史  

$ git log -n4 --onine  #简洁的查看最近四次的变更历史 

$ git log --all  #查看所有分支的提交

$ git log --all --graph  #图形化查看所有分支的变更

$ git log --n4 --graph  #图形化查看所有分支的变更
```

### 使用gitk图形界面

```bash
$ gitk --all 
```

### .git文件

如果你不想让git在管理你的项目直接删去.git文件就行了. 

> HEAD:文本文件

里面的内容是指向正在使用的分支的顶端的指针,指向master或者其他分支,切换分支的时候就会改变他的值

当你切换分支的时候,内容就会发生变化

```C
refs/heads/master
```

> config

包含与本地仓库相关的配置信息

包含远程url,邮箱.使用 git config 进行修改文件的内容

> refs

![image-20200425225250367](/home/zhangeaky/.config/Typora/typora-user-images/image-20200425225250367.png)

- heads 目录下面保存的是各个分支最近的一次commit的哈希值


![image-20200425235507203](/home/zhangeaky/.config/Typora/typora-user-images/image-20200425235507203.png)

- tags 里程碑,不做特别的标注是没有内容的

> Discription

显示仓库的描述

>  hooks

在commit,rebase,pull时可以自动运行.

>  **objects**

### commit/tree/blob三个对象的关系

> **commit** : 变更

>  **tree **:每一个commit只对应一个**tree**,描述的是对应的commit下所有的文件和文件夹之间的关系的图谱.

>  **blob**: 指的是具体的某一个文件(与文件名无关,只与文件内容相关,两个文件的文件内容相同,就完全相同)

```bash
git cat-file -p [哈希值]   查看文件内容

git cat-file -t [哈希值]   查看文件类型
```

如果有文件被加入到暂存区,git就会创建出object管理我们的文件.

HEAD和BRANCH

$ git checkout -b [fix_readme] [fix_css]  基于fix_css分支创建新分支fix_readme

HEAD可以不指向任何一个分支,在我们创建一个新的分支或者切换到一个新的分支的时候,HEAD就指向那个分支. 

 

$ git diff HEAD HEAD^1

 

 

 

 

 

 

 

 

 

 

 

 

 

 

修改commit中的message

$ git commit --amend 对最近一次提交的commit中的message做变更.

$ git rebase -i [要改的commit的上一次commit的哈希值]

会进入一个文本文件,修改message前面的关键字 pick->r,并进行保存(ctrl+o)按下回车键,退出后会进入一个文件显示着原有的message的内容,修改并保存退出.

 

将几个commit整理成一个

将连续的几个commit整理成一个

$ git rebase -i [连续几个的第一个的前一个commit的哈希值]

会进入一个文本文件,将目标的pick改成s.必须保留最早的一个commit将其他	的整理成为最早的那一个commit.

 

将不连续的几个commit合并成一个

$ git rebase -i [哈希值]

将要粘合的两个commit写在一起,并将靠后的commit的pick改成s

 

 

 

 

 

 

临时加塞紧急任务

$ git stash    把当前的任务封存到stash中

$ git stash list  查看stash列表中的信息

$ git stash apply 把之前存放的任务弹出来,暂存区;stash list 这条工作任务的信息依然被保存

$ git stash pop 把之前存放的任务弹出来,暂存区;stash list 这条工作任务的信息不被保存

指定不需要git管理的文件( .gitignore)

 

你在工作区创建的’.gitignore’文件的文件名必须就为’.gitignore’.

写入doc则git不会管控doc文件,写入doc/则git不会管控doc文件夹下的文件

 

 

 

 

### git备份

(一)本地协议

哑协议:传输进度不可见

智能协议:传输进度可见,传输速度快

$ git clone --bare [本机下的工作路径/.git]  ya.git

$ git clone --bare [file:// 本机下的工作路径/.git]  zhineng.git

$ git remote add zhineng file://路径/.git

$ git push 

GitHub公私钥

### 本地项目上传GitHub

>  配置生成公私钥

```bash

```

>  将公钥复制粘贴到github网站的profile中.

```bash
git remote add [your own setted name] [ssh协议从网页复制(或者http)]
```

> 其他命令

```bash
git remote -v :查看远端备份站点

git remote remove [name] 删除远方站点

git push [github] --all
```

> 远端为非空仓库

```bash
$ git fetch [远端名] [分支] 将远端的分支拿来

$ git merge --allow related histories [远端名]/[分支]

```



 在本地新创建分支时, 第一次向远程push的时候要执行一下命令,不能简单直接push

```bash
git push origin ${new_branch_of_remote}
```



 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 