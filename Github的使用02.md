# github的使用02

github是一款使用git命令作为基础框架的网站，它是一款开源分享网站，你开源把你的源代码放到github上，然后让人来start给你小星星，小星星越多代表你的项目越具有影响力，很多公司面试如果你有一个很多星星的项目，会大大提升你的录取率。

你也可以把你的一些项目分享到github上保存，github上是无限制代码的。

1.首先到github上注册一个你的账号

2.在本地创建一个ssh的key，因为github是使用ssh服务进行通讯的

```bash
ssh-keygen -t rsa -C "2218201514@qq.com"
```

-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名，一般我们默认，让存储到默认路径以及默认文件名

它会要求输入Enter file in which to save the key (/home/gec/.ssh/id_rsa)

这里是生成的sshkey文件名，我们可以回车使用默认文件名

除此之外还会让你输入

Created directory '/home/stephenzhou/.ssh'.
Enter passphrase (empty for no passphrase): 
这个密码会在让你push提交时候要输入的，除了git登录密码，还要输入这个密码，直接回车则空密码，这里我们直接回车

接着会让你在此输入密码，验证这里依旧回车

Enter same passphrase again：

生成之后你就会看到这样的界面:

![image-20250713205155955](https://gitee.com/liweihanNB/typora/raw/master/20250713205156027.png)

生成的ssh文件如果不使用-f指定的话会生成在用户目录下的.ssh目录中，.ssh是隐藏文件，可以使用ls -ah看到，使用cd ~进入用户主目录，然后cd进入到.ssh目录中可以看到文件

![image-20250713205242502](https://gitee.com/liweihanNB/typora/raw/master/20250713205242547.png)

id_rsa是私匙，id_rsa.pub是公匙，id_rsa不能告诉任何人，只有公钥可以，ssh采用的是非对称加密。

接着在github上添加你的公钥

![image-20250713205725534](https://gitee.com/liweihanNB/typora/raw/master/20250713205725679.png)

![image-20250713205946184](https://gitee.com/liweihanNB/typora/raw/master/20250713205946330.png)

最后在输入你的登录验证或密码就可以了

![image-20250713210103463](https://gitee.com/liweihanNB/typora/raw/master/20250713210103506.png)

这样ssh就添加成功了~

![image-20250713210148098](https://gitee.com/liweihanNB/typora/raw/master/20250713210148144.png)

你可以添加如很多个ssh，比如你有多台电脑，在每个电脑上都配置ssh然后添加进来就可以了，git需要这个是要确定你是主人，确定是主人的机器推送的才可以推送到仓库中，但是你可以创建公开仓库，别人只能拉取不能推送到这个仓库中，你可以给其它人权限。

找到你要开放的仓库，选择Manage access然后使用invite a cikkaborator添加成员就可以了。
![在这里插入图片描述](https://gitee.com/liweihanNB/typora/raw/master/20250713210421680.png)

## github上创建仓库

我们可以在github上创建一个仓库

![image-20250713210501204](https://gitee.com/liweihanNB/typora/raw/master/20250713210501243.png)

创建时记得选上readme文件，因为这个文件是github上的md文件，用来显示项目简介的，建议选上，日后我会教大家如何去写md文件，或者可以去使用一些在线的md文件生成网站也可以。

![image-20250713210758119](https://gitee.com/liweihanNB/typora/raw/master/20250713210758256.png)

创建完成之后就是这个样子的

![image-20250713210819193](https://gitee.com/liweihanNB/typora/raw/master/20250713210819341.png)

什么也没有，只有一个readme文件

## github将本地仓库关联到远程仓库：git remote add origin

我们本地有一个仓库，我们想把它推送到远程上去，很简单，我们只需要使用git remote add origin命令就可以了，ongin是github上的仓库名称，意思是远程仓库的意思。

首先选择仓库的code找到github生成的远程仓库链接

![image-20250713211152313](https://gitee.com/liweihanNB/typora/raw/master/20250713211152359.png)

然后关联

```bash
git remote add origin git@github.com:beiszhihao/test.git
```

然后使用git push推送到远程

```bash
git push -u origin master
```

> push：将本地仓库与远程仓库合并
>
> -u：将本地仓库分支与远程仓库分支一起合并，就是说将master的分支也提交上去，这样你就可以在远程仓库上看到你在本地仓库的master中创建了多少分支，不加这个参数只将当前的master与远程的合并，没有分支的历史记录，也不能切换分支
>
> origin：远程仓库的意思，如果这个仓库是远程的那么必须使用这个选项
>
> master：提交本地matser分支仓库
>

这个时候你可以在github上看到有提交记录
<<<<<<< HEAD
=======

![image-20250713212847841](https://gitee.com/liweihanNB/typora/raw/master/20250713212915774.png)

但是什么都没有，因为这个分支是main，我们提交的是master

选中它然后切换到onlyTT5

![image-20250713213017558](https://gitee.com/liweihanNB/typora/raw/master/20250713213017598.png)

默认是没有onlyTT5的，这是我们新添加的分支

![image-20250713213055257](https://gitee.com/liweihanNB/typora/raw/master/20250713213055321.png)

看到有文件了。

github上已经默认是main作为主仓库了

## git将远程仓库关联到本地和拉取指定分支、切换远程分支：git clone

当我们远程有仓库时，想要关联到本地只需要使用git clone就可以了

新建一个空目录，不要git init

使用git clone会自动帮我们初始化

## github提交本地仓库到远程仓库：git add、git commit、git push

我们修改了master上的分支代码，然后使用git add提交到缓存区，在使用commit提交到本地仓库，在使用push推送到远程就可以了，非常简单，命令都是我们学过的

![image-20250720200654791](https://gitee.com/liweihanNB/typora/raw/master/20250720200701907.png)

## git修改分支名称：git branch

使用-m选项

git branch -m 分支名 新的分支名

## git保存当前工作切换分支：git stash

在你当前工作区修改了文件或者其它功能时，你想要切换或者创建到其它分区是不可能的，如：

![image-20250720200758877](https://gitee.com/liweihanNB/typora/raw/master/20250720200758914.png)

我们分支修改了内容，想要切换到其它分区git会终止你这样操作，为的是防止丢失当前工作区内容。

我们可以使用git stash命令来保存当前工作状态

```bash
git stash
```

保存工作状态之后可以使用git stash list查看当前存储了多少工作状态

```bash
git stash list
```

![image-20250720201036481](https://gitee.com/liweihanNB/typora/raw/master/20250720201036526.png)

当在别的分支做完事情之后，在切换回刚刚的分支，然后在刚刚的分支中将状态恢复

```bash
git status pop
```

![image-20250720201139319](https://gitee.com/liweihanNB/typora/raw/master/20250720201139366.png)

git stash pop会将list保存的列表也给删除掉

git stash apply 不会删除列表里的内容会默认恢复第一个

如果想恢复指定内容可以使用git stash apply list名称

git stash drop list名称可以移除指定list

git stash clear 移除所有lsit

git stash show 查看栈中最新保存的stash和当前目录的差异。

注意stash是以栈的方式保存的，先进后出。

准确来说，这个命令的作用就是为了解决git不提交代码不能切换分支的问题。

## git远程删除分支后本地git branch -a依然看得到的问题：git remote 

这个问题是因为本地没有更新分支缓存

可以使用remote命令对远程仓库进行操作

使用 `git remote show origin命令查看远程仓库信息`

```bash
 git remote show origin
```

![image-20250720201445295](https://gitee.com/liweihanNB/typora/raw/master/20250720201445345.png)

## git强制合并分支：--allow-unrelated-histories

当我们在使用两个不同的分支时或此分支不是从原生仓库中分支出来的，想要合并不符合GIT规则，所以会弹出：fatal: refusing to merge unrelated histories 的错误，比如当我们在本地开发好了，但是并没有在一开始关联远程仓库，若想提交就会出现这样的错误，我们先拉取下来以后合并分支在后面加上这条语句就可以了

```bash
git merge master --allow-unrelated-histories
```

## Git新增分支操作：git switch、git restore

这两个命令是git 2.23以后引入的命令，目的是为了提供对新手更友好的分支操作，最早我们使用的是git checkout命令来对分支进行操作，这个命令相对于复杂了许多，使用很多子参数来进行操作，为此git新增了两个命令：switch、restore，switch是用来切换分支与新增分支的，而restore用来撤销文件的修改，使其变得更明确一点
切换分支：

```bash
git switch dev
```

注意如果分支不存在，是不会创建的

切换到commit ID：

切换到指定id并创建一个分支，我们称之为分离HEAD状态

```bash
git switch -d f8c540805b7e16753c65619ca3d7514178353f39
```

只需要加上-d参数就可以了，而checkout是不需要加-d的，在switch里一切变得明确了很多

如果要合并一个分支必须加上-b

```bash
git switch -b dev
```

创建分支则是-c

```bash
git switch -c dev
```

git restore命令是用来撤销提交与修改的，如：

```bash
git restore file
```

使用这条命令会将文件从暂存区删除

```bash
git restore file
```

这条命令会不会将文件从暂存区里删除，会将文件在暂存区里的状态覆盖到工作区，如我在工作区对这个文件又进行了修改，那么使用这个命令可以将这个文件在暂存区里的内容恢复到工作区
>>>>>>> develop
