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
