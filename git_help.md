# 安装Git
## 在Linux上安装Git



在`Ubuntu`系统中，只需要通过一条指令就能安装`git`
```bash
sudo apt-get install git
```



## 创建版本库
版本库英文名`repository`，相当与一个==目录==，这个目录里的一切都能被`git`所监管
创建一个版本库十分简单，首先选择一个合适地方，创建一个空目录
```bash
mkidr learngit
cd learngit
```



接着将这个目录仓库化
```bash
git init
```



仓库化后会在目录下创建`.git`的隐藏目录，作为一个仓库的管理目录，至此仓库初始化完成



## 把文件添加到版本库



首先明确一点，所有的版本控制系统，其实只能跟踪文本文件的改动，这些文件包括：
- `TXT`文件
- `HTML`网页文件
- 代码



不包括二进制文件：
- 图片
- 视频
- 游戏



不幸的是，`Microsoft`的`Word`格式是二进制文件，因此，版本控制系统是无法跟踪`Word`文件的改动的，因此推荐以纯文本方式编写文件



==使用Windows的开发者要特别主题:==
千万不要使用`Windows`自带的笔记本编辑任何文件，这都不能被`git`所有效管理，建议下载`Notepad++`代替记事本，不但功能强大，而且免费



我们首先编写一个`readme.txt`文件
```bash
Git is a version control system.
Git is free software.
```



添加文件分为两部操作:
- 第一步，用`add`告诉`git`,把文件添加到缓冲区
```bash
git add readme.txt
```



- 第二步，用`commit`告诉`git`, 把文件正式提交到仓库
```bash
git commit -m "worte a readme file"
```



这里`-m`的作用在于为此次操作添加标志信息，方便在后续浏览历史记录



# 时间漫游
## 版本回溯



在学习回溯之前，我们首先要有至少两个版本
于是对`readme.txt`进行如下修改
```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
```



接着提交，方法和上一节一样，不做赘述
`commit`相当于游戏中的存档，你在哪里卡住了，想要读取前一个存档，都需要它的支持
截至到目前，我们有三个版本的文件，想要便捷地查询历史记录，我们可以
```bash
git log
```


从记录里可以看到每一个版本的提示信息及其==版本号==，如果嫌输出信息太多，可以加上`--pretty oneline`选项



在`git`中，`HEAD`指针指向的就是当前版本
上一个版本是`HEAD^`
上上一个版本是`HEAD^^`
n个之前的版本是`HEAD~n`



我们用`reset`命令进行版本回溯
```bash
git reset --hard HEAD^
```



如果想要进行逆向过程，即版本跃迁，必须要知道跃迁版本的版本号
```bash
git reset --hard id
```



提供一个方法，`git log`查看历史记录时，将该进程挂起，再下一次就可以直接把该进程调至前台获得版本号



当然系统提供了后悔药
```bash
git reflog
```



## 工作区和暂存区



暂存区是`git`区别于其他操作系统的主要区域



- 工作区
工作区就是你在电脑里能看见的目录



- 版本库
`git`的版本库就是工作区的隐藏目录`.git`
这个库里存放了很多东西，其中最重要的有
1. 名为`stage`的暂存区
2. `git`为我们自动创建的第一个分支`master`
3. 控制指针`HEAD`



![](https://lvxiaojun11.oss-cn-beijing.aliyuncs.com/img/20201211141910.png)



前文所说的文件添加步骤可以细化到以下两步
- 第一步是用`git add`把文件添加到暂存区
- 第二步是用`git commit`把暂存区的内容推送到当前分支



想要验证这个机制，可以用库状态查询指令
```bash
git status
```



## 管理修改



==管理修改==是`git`比其他版本控制系统优秀的重要原因



通过设置工作区与缓冲区实现文件分段式存储，我们可以用`diff`命令查看工作区和版本库里的文件的区别
```bash
git diff HEAD -- xxx.txt
```



## 撤销修改



当有一天凌晨，你不小心在程序文件里写上了一句辱骂老板的话，别急我们有后悔药
```bash
git checkout -- xxx.txt
```



根据情况的不同`checkout`有不同的效果
- `xxx.txt`自修改后还没有被放到缓存区，那么，撤销修改就回到和版本库一模一样的状态
- `xxx.txt`已经被添加到缓存区，又做了修改，现在，撤销修改就回到添加到暂存区后的状态



值得注意的是，`--`十分重要，没有`--`，就变成了“切换到另一个分支”的命令



上文中提到了`reset`命令可以进行版本回退，在文件从暂时存区提交到分支之前同样可以用来恢复文件
```bash
git reset HEAD xxx.txt
```



## 删除文件



在`git`中，删除也是一个修改动作，对于删除普通的文件，我们可以直接
```bash
rm xxx.txt
```



这个时候，删除的只是工作区的文件，而没有删除版本库中的文件
现在你有两个选择
1. 如果确实想删除该文件直接`git rm xxx.txt`,然后`git commit -m "..."`即可
2. 如果是误删，因此文件在版本库中还留有备份，可以用`git checkout -- xxx.txt`命令将其还原



# 远程仓库
## 添加远程仓库



在本地创建`git`仓库之后，又像在`github`创建个人仓库，并且让这两个仓库进行远程同步
首先，登录`github`创建一个个人仓库
目前这个仓库是空的，`github`支持两种操作:
- 从这个仓库克隆新的仓库到本地或是别的用户家目录下
- 把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到`github`



首先根据`github`的提示，在本地进行链接
```bash
git remote add origin git@github.com:yourId/yourRepositoryId.git
```



这样就在本地创建了远程仓库的映射`origin`，当然你也可以取别的名字
`yourId`为你的`github`帐号名
`yourRepositoryId.git`为你创建的仓库的名字



注意这里需要针对`SSH`协议配置免密登录，操作和免密登录服务器一致，这里不做赘述



链接建立完后我们就可以把本地库的所有内容推送到远程库上
```bash
git push -u origin master
```



在初次推送信息时，需要加`-u`选项，这里`git`不仅会把本地的`master`内容推送到远程的`master`分支，还会将两者关联起来，在以后的推送或是拉取时就可以简化命令


注意新版的`github`把默认分支从`master`改成了`main`，这里可以在网站修改默认分支为`master`，上述命令便可以正常使用



这里额外介绍如果修改分支名
```bash
git branch -m oldname newname
git branch -m newname
```



如果`-m`选项后只一个`name`，就默认修改当前分支名为指定名



从现在起，想要推动信息，只需要
```bash
git push origin master
```



或者用更为省略的版本
```bash
git push
```



推送当前分支到远程仓库的同名分支



## 从远程仓库克隆



现在我们从0出发，从远程仓库克隆到本地仓库
首先在`github`上新建一个带有`README`文档的仓库
接下来用`clone`命令克隆一个本地库
```bash
git clone git@github.com:yourId/yourRepositoryId.git.git
```



此时在当前目录下就已经创建了同名文件，这是一个已经配置好的仓库



# 分支管理
## 创建与合并分支



`master`是我们默认创建的分支，即主分支
严格意义上来讲，`HEAD`并不指向提交，而是指向当前分支，当前分支才指向提交



首先我们可以用`branch`命令创建一个分支
```bash
git branch branchName
```


接下来用用`checkout`命令切换到创建的分支
```bash
git checkout branchName
```



当然这两部可以简化为一个命令
```bash
git checkout -b dev
```



然后查看当前分支
```bash
git branch
```


`*`所指向的分支就是我们的当前分支



接着我们就可以在自己的分支上愉快地玩耍，耍完之后按照之前的两部提交即可
切换回`master`分支，发现之前的修改并没有作用在这个分支，现在我们想把成果合并到这个分支上
```bash
git merge branchName
```



至此修改也同步到了`master`分支
如果信息中含有`Fast-forward`,就代表是快进模式，这个模式直接修改`master`指针的指向，所以合并速度非常快



之后我们还需要释放多余的`branchName`指针，删除这个分支
```bash
git branch -d dev
```



如果你觉得之前的语法不好记忆，我们还有更科学的操作
```bash
git switch -c dev
```



这个命令可以创建并切换到新的分支，如果想要切换到已有的分支，可以
```bash
git switch master
```



## 解决冲突



如果两个分支同时对文件做了修改，并且在合并是不是简单地叠加，在`merge`时就会出现问题

![](https://lvxiaojun11.oss-cn-beijing.aliyuncs.com/img/20201211160548.png)



在报错信息中会指明出现冲突的文件，我们修改制定文件即可

![](https://lvxiaojun11.oss-cn-beijing.aliyuncs.com/img/20201211160707.png)



在文件中`git`用`<<<<<<<`,`=======`,`>>>>>>>`标记出不同分支的内容，我们直接==人工判定最终版本即可==



两部走提交，现在分支关系就发生了变化

![](https://lvxiaojun11.oss-cn-beijing.aliyuncs.com/img/20201211161000.png)

用`git log`命令也可以看到分支的合并情况
最后删除新创建的分支即可
```bash
git branch -d branchName
```


我们也可以查看分支合并图
```bash
git log --graph
```


## 分支管理策略



一言以蔽之，`--no-ff`可以帮助我们保存合并的历史
```bash
git merge --no-ff -m "merge with no-ff" branchName
```



这样的好处在于使用`git log`命令可以查看完整的合并历史
