# Git的使用

[toc]

## Linux安装Git

```bash
sudo apt install git
```

## 配置git环境：git config --global

> config：参数是用来配置git环境的
>
> --global：长命令表示配置整个git环境

### 用户名配置

> user代表用户，.name代表配置用户的名称

```bash
git config --global user.name "你的用户名"
```

### 邮箱配置

> user代表用户，.email代表配置用户的邮箱

```bash
git config --global user.email "你的邮箱"
```

## 创建本地空仓库：git init

使用git init初始化当前仓库

```bash
git init
```

初始化后会生成git的配置文件目录，普通的"ls"命令是看不到的，我们需要使用ls -ah查看隐藏目录

![image-20250712160416115](https://gitee.com/liweihanNB/typora/raw/master/20250712160416269.png)

## 新建文件添加到本地仓库：git add、git commit -m

使用git add命令将文件添加到本地仓库的提交缓存

```bash
git add test.c 
```

这个时候还不算添加到了本地仓库，我们还需要使用git commit命令为其添加修改的描述信息

我们需要使用-m命令来简写描述我们的信息，如果不使用-m，会调用终端的注释编辑器让你输入描述信息，但是不建议使用，因为注释编辑器比较难用，不舒服。

```bash
git commit -m "add new file \"test.c\""
```

至此，这个文件就已经添加到本地仓库中了，同时本地仓库也迭代了一个版本。

### 改写提交：git commit --amend

> --amend：重写上一次的提交信息

发现注释写错了，我们可以使用 --amend长命令选项来改写提交

```bash
git commit --amend
```

可以看到刚刚的注释信息

在界面中按下“i”即可进入编辑界面

修改完成后按下ctrl+o键

按下ctrl+x(不分大小写)即可退出编辑界面

![image-20250712162147067](https://gitee.com/liweihanNB/typora/raw/master/20250712162147148.png)

## 查看历史提交日志：git log

> log：查看日志

```bash
git log
```

![image-20250712162354652](https://gitee.com/liweihanNB/typora/raw/master/20250712162354695.png)

第一行的commit是哈希算法算出的id，正如一开始所说，分布式是没有一个主版本号的，它们都是用id来做标志的，同时用master作为主仓库，其它的分支怎么迭代都不会影响到master，后面我会介绍如何使用分支

目前我们的仓库就是master，因为我们没有拉取分支是直接用git init创建的，就是master。

```bash
commit f4f0d9dd62d9161110b14620a02ec031c207dc49 (HEAD -> master)
```

后面的head是指向的意思，表示这次提交到哪儿，head->master代表这次提交到master主仓库，如果是head->分支仓库则代表提交到分支仓库

Author是提交者是谁的意思，显示格式是：用户名 <邮箱>

```bash
Author: onlyTT5 <2218201514@qq.com>
```

Date的意思是提交时间，后面的+0800这个是格林尼治时间，代表当前是以哪儿的时间地作为基准，这是世界时间，用它来作为基数与当前所在地时差进行计算，包括地球自转等公式。

```bash
Date:   Sat Jul 12 16:06:14 2025 +0800
```

最下面的就是注释了

```bash
test add new file "test.c"
```

## 回滚代码仓库：git reset --hard

> reset参数是重置命令
>
> --hard是重置代码仓库版本

--soft 、--mixed以及--hard是三个恢复等级。

使用--soft就仅仅将头指针恢复，已经add的暂存区以及工作空间的所有东西都不变。
如果使用--mixed，就将头恢复掉，已经add的暂存区也会丢失掉，工作空间的代码什么的是不变的。
如果使用--hard，那么一切就全都恢复了，头变，add的暂存区消失，代码什么的也恢复到以前状态。

1.回滚到指定历史版本

先使用git log查看历史版本

```bash
git log
```

在使用git reset --hard命令回滚

```bash
git reset --hard 要回滚id
```

实例：

使用git log回滚

第一行的commit后面的字符串就是我们的哈希id

![image-20250712164012172](https://gitee.com/liweihanNB/typora/raw/master/20250712164012214.png)

2.回滚当前仓库指向的版本

上面我们说过，HEAD是指向当前仓库的，历史版本中可能有别的分支，我们只想迭代我们仓库的上一个版本，这个很简单，我们只需要用HEAD来指向就可以了

```bash
git reset --hard HEAD^
```

^代表上一个版本的意思，HEAD代表当前仓库的指向，当前HEAD指向master，就代表回滚到master上一次提交的版本

当然我们也可以使用另外一种方式来回滚到当前仓库的指定版本

```bash
git reset --hard HEAD~3
```

后面的~3，代表以当前版本为基数，回滚多少次。HEAD~3代表回滚master前三个版本

如果觉得log打印内容过多，可以加上--pretty=oneline选项简洁输出

![image-20250712164346220](https://gitee.com/liweihanNB/typora/raw/master/20250712164346263.png)

## 查看提交之后文件是否做了改动：git status

> status：查看当前仓库状态

我们在提交完成之后，有时候可能自己不小心改动了某个文件，或者别人，我们可以使用git status查看文件是否被改动

我们修改一下刚刚提交的test.c文件，在里面随便输点字符

保存退出，然后使用git status查看

![image-20250712165057424](https://gitee.com/liweihanNB/typora/raw/master/20250712165057473.png)

可以看到报出了修改，这里我的环境语言是中文，如果是英文则对应的修改是AM，A是未修改

如果你添加了新文件，git status一样会报出来

这里我们添加一个新的文件

```bash
touch min.c
```

然后使用status查看一下

![image-20250712170200117](https://gitee.com/liweihanNB/typora/raw/master/20250712170200169.png)

如果不是中文会在后面写一个Untracked代表未提交

我们使用git add提交到缓存区文件后，使用git status也可以查看到当前文件的状态

![image-20250712170244817](https://gitee.com/liweihanNB/typora/raw/master/20250712170244867.png)

对应的英文是：modified

英文对应：

A：未修改

AM：修改

Untracked：未提交

modified：新文件，但未提交

如果提交了的文件，且没有改动的，不会显示到这个里面

## 工作区与缓存区

在git下有一个概念是缓存区，这是其它集中式版本控制系统没有的

工作区：工作区就是你当前的工作目录

缓存区：这里存放了你使用git add命令提交的文件描述信息，它位于.git目录下的index文件中

![image-20250712170458530](https://gitee.com/liweihanNB/typora/raw/master/20250712170458576.png)

有的低版本中叫stage

这些文件中存储了我们一些提交的缓存数据，git会解析它们，HEAD文件就是指向当前的仓库

最后使用git commit提交时git会提交到当前仓库中，当前的工作区也就成为了最新一次提交的仓库版本。

### 修改缓存区内容：git add、git commit -m

比如我们使用git add添加了一个名为min.c的文件，但是还没有提交的时候我们修改了它的内容，修改完成之后再提交会发现内容并不是我们第二次修改的内容

这就要说一点，当我们使用git add添加到缓存区的内容后，我们再修改这个文件时，它跟缓冲区内容是没有任何关系的！我们使用git commit提交的时，它只会提交缓存区内容

如果想提交第二次修改，我们只需要在git add一次，然后在使用git commit提交就可以了，git会自动帮我们合并提交

实例：

1.将文件添加到缓存区中

```bash
git add min.c
```

2.修改文件内容

3.在此添加到缓存区

```bash
git add min.c
```

4.提交

```bash
git commit -m "add min.c"
```

### 将改动文件添加到缓存区：git add

平时我们可能写代码的时候不可能保证只改动了一个文件，我们切来切去最后都不知道自己改了哪些文件，为了保证所有的文件都能被准确提交，我们可以使用git add我们确定修改的文件，当git add后在使用status查看一下状态，看看是否有遗漏没有提交的文件：

![image-20250713091220457](https://gitee.com/liweihanNB/typora/raw/master/20250713091220532.png)

可以看到test.c没有提交，在使用git add将test.c添加进来就可以了

### 将所有改动文件添加到缓存区：git add --all、git add .

如果你实在不确信哪些文件是改动过的，你只需要使用git add --all

```bash
git add --all
```

这个命令会将当前目录下包括子目录下所有改动的文件提交到暂存区，注意只包括改动的文件，不改动的不会放到缓存区。

这个命令还会把删除的文件也提交进去

如你在本地删除了min.c 这个命令会把删除信息也记录进去，然后在提交的时候把仓库里对应的min.c也删除掉，也就是说你在本地做的删除操作会被记录，提交仓库时会删除同样的文件，如果不想删除文件，可以使用git add .，注意后面有一个“.”点的符号，这个命令跟git add --all一样，但是不会记录删除操作。

最后别忘记使用git commit提交到仓库中

## 将文件撤销回到最近一次修改的状态：git checkout -- file

> checkout：切换参数，通常用来切换分支仓库

当我们在工作中修改了一个文件，猛然间发现内容好像改的不对，想重新修改，这个时又不知道自己改了什么代码，想撤销修改，有一个最简单的方法，就是git checkout -- file，注意中间要有“--”，checkout这个命令是切换分支的功能，关于它我们后面在细说，你现在只需要知道这个命令加上“--”可以用来将文件切换到最近一次的状态

注意这个恢复只能恢复到上一次提交的状态，如你刚提交了这个文件到仓库，随后你修改了它，那么使用这个命令只会回到刚刚提交后的那个状态里，不能回到你还没有提交，但修改的状态中。

下面这个演示，我将min.c文件修改了，并使用git checkout -- file回到了之前修改的状态

![image-20250713092047916](https://gitee.com/liweihanNB/typora/raw/master/20250713092047990.png)

注意这个功能不能一直迭代恢复，如你恢复到了修改前的版本，你想再次回滚回滚到修改前在之前的版本是不行的。

## 查看单个文件可回滚版本：git log filename

当我们想回滚指定文件到指定版本时，需要查看该文件有多少个版本可以回滚时，可以使用git log filename命令

```bash
git log test.c
```

![image-20250713093144343](https://gitee.com/liweihanNB/typora/raw/master/20250713093144433.png)

```bash
git log min.c
```

![image-20250713093220131](https://gitee.com/liweihanNB/typora/raw/master/20250713093220198.png)

```bash
git reset 0c4e937824a78f238f081829a2639528de84f750
```

![image-20250713093348882](https://gitee.com/liweihanNB/typora/raw/master/20250713093348951.png)

## 删除文件：git rm

如果我们使用普通的命令，rm删除文件，git状态会提示你删除了文件，你只需要使用add重新提交一次就可以了。

![image-20250713093848609](https://gitee.com/liweihanNB/typora/raw/master/20250713093848703.png)

当然你也可以使用git rm删除文件，但是也需要使用git commit提交一次

可以看下status的状态

![image-20250713094025903](https://gitee.com/liweihanNB/typora/raw/master/20250713094025978.png)

## 查看提交历史：git reflog

git reflog可以查看当前版本库的提交历史，凡是对仓库版本进行迭代的都会出现在这个里面，包括你回滚版本都会出现在这个历史中

![image-20250713094309631](https://gitee.com/liweihanNB/typora/raw/master/20250713094309775.png)

## git基本组成框架：Workspace、Index / Stage、Repository、Remote

> - Workspace：开发者工作区
> - Index / Stage：暂存区/缓存区
> - Repository：仓库区（或本地仓库）
> - Remote：远程仓库

Workspace：开发者工作区，也就是你当前写代码的目录，它一般保持的是最新仓库代码。

Index / Stage：缓存区，最早叫Stage，现在新版本已经改成index，位于.git目录中，它用来存放临时动作，比如我们做了git add或者git rm，都是把文件提交到缓存区，这是可以撤销的，然后在通过git commit将缓存区的内容提交到本地仓库

Repository：仓库区，是仓库代码，你所有的提交都在这里，git会保存好每一个历史版本，存放在仓库区，它可以是服务端的也可以是本地的，因为在分布式中，任何人都可以是主仓库。

Remote：远程仓库，只能是别的电脑上的仓库，即服务器仓库。


## git rm后恢复文件：git rm、git reset、git checkout

### 情况 1: 文件已被提交到 Git 仓库

```bash
git rm delete.txt
```

git reset重置缓存区

```bash
git reset []|[文件名]
```

git checkout 

```bash
git checkout delete.txt
```

![image-20250713103211777](https://gitee.com/liweihanNB/typora/raw/master/20250713103211863.png)

### 情况 2: 文件已被暂存但未提交

使用以下命令恢复文件：

```bash
git restore --staged <文件名>
```

然后：

```bash
git restore <文件名>
```

这会从暂存区恢复文件到工作区。

## git创建分支：git branch、git checkout

使用git checkout -b参数来创建一个分支，创建完成分支后会自动切换过去

```bash
git checkout -b dev
```

然后我们在使用branch来查看当前属于哪个分支，也就是查看HEAD的指向

```bash
git branch
```

![image-20250713110155617](https://gitee.com/liweihanNB/typora/raw/master/20250713110155691.png)

git checkout -b等价于

```bash
git branch dev
git checkout dev
```

git branch 如果后面跟着名字则会创建分支，但不会切换

git checkout 后面如果是分支名称则切换过去

## git切换分支：git checkout

当我们想切换分支可以使用git checkout来切换，如刚刚我们创建了一个分支dev并切换了过去，现在切换回masterk

```bash
git checkout master
```

![image-20250713110412862](https://gitee.com/liweihanNB/typora/raw/master/20250713110412934.png)

git checkout的作用是检出，如果是文件的话，会放弃对文件的缓存区操作，但是要使用reset重置一下变更才行。

如果是分支的话会切换过去。

## git合并分支：git merge

当我们新建分支并做完工作之后，想要把分支提交至master，只需要切换到master仓库，并执行git merge 分支名就可以了

如我们在分支中新建了一个dev1.txt和dev2.txt的文件，并提交

![image-20250713111713613](https://gitee.com/liweihanNB/typora/raw/master/20250713111713700.png)

![image-20250713111815301](https://gitee.com/liweihanNB/typora/raw/master/20250713111815382.png)

发现在dev分支下有master分支下的文件，而在master分支下看不到dev分支下创建并提交的文件

![image-20250713111919661](https://gitee.com/liweihanNB/typora/raw/master/20250713111919739.png)

然后在使用git checkout master切换到master

在使用git merge dev将其合并

![image-20250713112208361](https://gitee.com/liweihanNB/typora/raw/master/20250713112208440.png)

这里需要说一点，如果你在任何分支下创建文件，没有提交到仓库，那么它在所有仓库都是可见的，比如你在分支dev中创建了一个文件，没有使用git add和git commit提交，此时你切换到master，这个文件依旧存在的，因为你创建的文件在工作目录中，你切换仓库时git只会更新跟仓库有关的文件，无关的文件依然存放在工作区。

同时我们可以看到历史版本中有分支提交的历史

![image-20250713112237985](https://gitee.com/liweihanNB/typora/raw/master/20250713112238062.png)

## git查看分支：git branch -a

如果要查看当前所有分支可以使用：git branch -a

HEAD指向当前分支

![image-20250713112359879](https://gitee.com/liweihanNB/typora/raw/master/20250713112359943.png)

## git删除本地分支：git branch -D

```bash
git branch -D [分支名]
```

## git删除远程分支：git push origin --delete

注意这里的远程分支名不需要加origin，输入分支名就可以了

```bash
git push origin --delete [远程分支名]
```

## 在开发中git分支的重要性

当我们在开发中，无论做什么操作都建议使用分支，因为在团队开发中，master只有一个，合作开发里任何人都可以从master里拉取代码，拉取时master后创建分支，分支名改为你要做的操作，比如修改某某文件，修改什么什么bug，单词以下划线做分割，然后在提交一个版本

分支名必须简洁，和标题一样，提交的commit在简单描述一下就可以了。

如我们的master中有个bug，是内存泄漏

我们可以常见一个分支名为Memory_Leak,然后在commit里简单描述一下修复了哪个模块的内存泄漏，不要写修复了什么什么代码，什么什么问题导致的，只需要简单描述一下就可以了。

一般情况下，我们都是拉取master后，想要修改功能或者添加功能，都是创建分支，在分支里修改不影响master，如果修改错了代码或者误删之类的，在从master上拉取一份就可以了。

