# git使用指南

## 一.获得git仓库的两种方式

① 将尚未进行[版本控制](https://edu.csdn.net/cloud/sd_summit?utm_source=glcblog&spm=1001.2101.3001.7020)的本地目录转换为 Git 仓库

② 从其它服务器克隆一个已存在的 Git 仓库

以上两种方式都能够在自己的电脑上得到一个可用的 Git 仓库

## 二.三个区域

工作区
工作区，指的是使用Git管理后的文件，这些文件显示在磁盘上，供我们使用或修改的区域。所以，粗略的说，项目文件夹就是工作区。
暂存区
执行 git add .之后，文件由工作区，添加到了暂存区。 暂存区保存了下次将提交的文件列表信息。
本地仓库
执行 git commit -m ‘提交说明’ 之后，代码会被提交到仓库区。仓库区是 Git 中最重要的部分，代码只有提交到仓库，才会形成一次历史记录，即才会形成一个版本

## 三.git初始化

1. 查看文件的状态

   git status

2.  初始化，表示使用Git管理我们的项目。这个命令只需要执行一次（注意空格）

​		git init

​	执行 git init 命令之后，会在项目文件夹中生成一个隐藏的 .git 文件夹
​	.git 文件夹里面保存着当前项目文件的更改记录。所以这个文件夹不能删除
​	对于一个项目来说，git init 只需要执行一次
​	切记，不要项目套项目。

3. 查看日志的命令：

   git log

4.  查看文件状态

```
# 查看文件的状态
git status

# 查看简略版信息
git status -s
```

已提交（nothing to commit）
表示没有什么东西可以提交了；即所有的内容都已经提交过了
有的文档也把这个状态叫做 未修改，意思是自上次提交过后，代码还没有修改过
未跟踪（新增的文件）
已暂存（新增的文件，添加到暂存区之后的状态）
已修改（文件曾经被Git记录过了，然后在工作区对他进行了修改）
 初学者，只需要区分：代码是否都被提交到仓库了
![image-20241206220833542](D:\Desktop\Study_Note\image\image-20241206220833542.png)

切换分支之前，必须把当前分支的代码全部提交到仓库。

5. 合并分支
   特点：一个分支包含另一个分支的全部提交记录。

如果需要把dev分支的代码合并到master分支

6. 切换到master

执行 git merge add ，即可把add分支的代码合并到master

 合并之后，add和master分支的代码就回一样了。

合并提示
两个分支，比如是master和dev，特点是都有新的提交
也就是说，一个分支不包括另一个分支的全部提交记录
这种模式的合并，有可能会有冲突
合并方法，和前面一样
假设把dev的代码合并到master分支，切换到master git checkout master
执行 git merge dev ，表示将dev分支的代码合并到当前（master）分支

然后就会出现如下两个画面中的一种

- 画面一：两个分支修改的代码不在同一个文件的同一行代码，出现下面的画面。

![在这里插入图片描述](D:\Desktop\Study_Note\image\c1465d65fba47f60cd37aee3cf0aa4c6.png)

画面二：两个分支修改了同一个文件的同一行代码，出现下面的画面。

![在这里插入图片描述](D:\Desktop\Study_Note\image\da6875e7b735e78eaa7e6265b8b57d2c.png)

出现上述画面一，表示已经合并完成了，但是需要提交一次；
出现的框是让我们输入提交说明；

需要执行下面的操作：

按 i ，进入 “插入” 模式，就可以对画面中的文字进行修改了（直接输入也行）
按 “上下左右” 键，调整光标的位置，可以删除里面的内容，写自己的提交说明
上述画面中的 # Please enter… 表示注释，可以不用理会
按 Esc 键，退出 “插入” 模式
直接输入 “:wq”，退出这个画面，从而完成合并。（一定是英文的冒号）
出现上述画面二，表示正在合并中，但是遇到冲突了；

需要在代码中解决掉冲突，然后保存代码；

最后，需要提交一次；

具体做法：

打开有冲突的文件
去掉分割线，保存代码，表示解决了冲突
保存代码，执行 git add . 和 git commit -m ‘提交说明’ 从而完成这个合并。
## 四.创建远程仓库


1. github
   右上角的 “+” ，选择 “New repository”
   填写仓库名称
   点击创建按钮，创建。
   推送代码到远程【首次】
   进入本地项目文件夹，右键 --> Git Bash Here，打开终端窗口。
   远程仓库地址有两个（https、ssh地址），一定要选择ssh地址。
   添加远程仓库地址（git remote add 远程仓库地址别名 完整的远程仓库SSH地址）
   首次推送代码到远程仓库（git push -u origin master）
   只能把本地仓库的代码推送到远程仓库；不能把工作区的、暂存区的代码推到远程。
   确保你的本地仓库有内容，别推送空的本地仓库。注意是本地仓库，不是工作区。
   再次及后续推送
   工作区编写代码
2. 执行 add 命令，将代码添加到暂存区
   执行 commit 命令，将代码提交到本地仓库。（因为只有本地仓库的代码才能推送到远程）
   执行 git push 命令，将这次改动推送到远程仓库。
3. 常见的问题
   执行完“git remote add origin ssh地址”报错：fatal：remote origin already exists.
   叫做 origin 的远程地址以及存在了。不能再使用这个名字了。
   执行 git remote -v 查看所有的远程地址。
   可以选择性的 删除 远程地址。执行 git remote remove 别名，比如 git remote remove origin
   首次推送的时候，提示输入账号密码
   因为添加远程地址的时候，填错了，填成了 https 地址了。应该换成 ssh 地址。
   可以选择改过来，也可以使用https地址。下次做项目，再使用ssh地址也可以。
   git remote add 那里，地址复制错了，能改吗？
   ![image-20241206221123642](D:\Desktop\Study_Note\image\image-20241206221123642.png)

## 多人协作

1. 管理员角色
   创建远程仓库 或 创建本地仓库之后推送到远程仓库
   初始化一个项目，git init
   添加初始的代码到暂存区 git add .
   提交初始的代码到本地仓库 git commit -m “提交了初始的代码”
   推送到远程仓库
   git remote add origin SSH地址
   git push -u origin master
2. 邀请成员
   开发（add / commit / pull / push）
   编辑自己的代码
   把修改后的代码，添加到暂存区 git add .
   把修改后的代码，推送到本地仓库 git commit -m “xxx”
   如果有人在你之前推送了，则推送之前需要先拉取，将拉取下来的代码和你的代码合并 git pull origin master
   合并如果有冲突，需要解决冲突，别忘记提交一次
   最后推送 git push origin master
3. 成员角色
   同意邀请
   克隆项目到本地（注意路径）
   执行 git clone SSH地址 ，将项目克隆到本地。然后关闭黑窗口。
   进入项目文件夹，重新 git Bash Here 打开黑窗口，这样可以保证路径正确。
4. 开发（add / commit / pull / push）
   编辑自己的代码
   把修改后的代码，添加到暂存区 git add .
   把修改后的代码，推送到本地仓库 git commit -m “xxx”
   如果有人在你之前推送了，则推送之前需要先拉取，将拉取下来的代码和你的代码合并 git pull origin master
   合并如果有冲突，需要解决冲突，别忘记提交一次
   最后推送 git push origin master
   分支和远程相关命令
   查看远程分支
   ![image-20241206221210978](D:\Desktop\Study_Note\image\image-20241206221210978.png)

##### 跟踪分支

跟踪分支就是把远程仓库的分支下载到本地



![image-20241206221230989](D:\Desktop\Study_Note\image\image-20241206221230989.png)

git忽略
在项目中，创建 .gitignore 文件，它就是git的忽略文件，记录了哪些文件不被Git管理。

如果有的文件已经被Git管理了，而又想设置为忽略文件，则需要使用 git rm --cached 文件 将文件从仓库中移除才有效。

被成功忽略的文件，不会被添加到暂存区，不会被提交到本地仓库，不会被推送到远程仓库。这就是忽略的意思。
![image-20241206221250757](D:\Desktop\Study_Note\image\image-20241206221250757.png)



## 五.总结：
Git 中基本命令的使用
git init
git add .
git commit –m “提交消息”
git status 和 git status -s
Git 分支的基本使用
git branch 查看分支
git checkout 分支名称
git checkout -b 新分支名称
git merge 分支
git push -u origin 新分支名称