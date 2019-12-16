---
layout: post
title: Git 的使用
date: 2019-12-16
categories: Git
tags: Git
---

https://www.bilibili.com/video/av35112720

目录
==
[TOC]
## 新建本地仓库
- 新建一个文件夹，在文件夹中右键git bash here
- 输入 git init，会新建一个.git的隐藏文件夹
- 可以输入 git status 查看git状态，可以看待当前在master分支
- 新建a.txt,再次输入git status，可以看到显示有一个没有被跟踪的文件
![image](https://note.youdao.com/yws/res/26337/7F714DF5B9A54DA4A190C3A2C6AAD8EF)
- 输入 git add a.txt，提示改变可以被提交,此时这个文件需要再一次被审核
![image](https://note.youdao.com/yws/res/26340/54761935CC08414E81D34AB0B7E5EC52)
-  输入 git commit -m "new file a.txt", -m 代表把自己的注释加入进去,显示第一次提交，后面是自己写的注释
![image](https://note.youdao.com/yws/res/26351/341A0867990C4D48938773836664A1FA)
- 可以输入 git log 看一下自己在git上面的操作历史，HEAD表示版本，master是分支，两者合起来表示位置作者，时间，和自己的工作注释。
![image](https://note.youdao.com/yws/res/26358/BDF98BBAA96D46A9A1D02979D19AF3C2)

## 回退又见回退
### 直接回退一步
- 修改上面的a.txt文件，增加java is the best一行哈哈,输入 git status
![image](https://note.youdao.com/yws/res/26369/AEA6B184214C475D8382A5695F3C762C)
- git add . ，将暂存中的文件都加进去
- git commit -m "add new line"提交
- 输入 git reset --hard HEAD^ 回退一步，就是把输入的一行删掉，注意在这里增加了--hard 这样的话就不需要git checkout 的操作了。
![image](https://note.youdao.com/yws/res/26380/E5375FA7BEFD4406AB9AE36E092D8A0D)

### 根据版本号回退
- 可以输入 git log a.txt 来看看之前的commit，看看回到哪里，然后复制之前的版本号
![image](https://note.youdao.com/yws/res/26392/AD13527E4D834BFDA35D36B238891D86)
- git reset 版本号,注意此时没有改变，这里没有--hard操作，后面要checkout
![image](https://note.youdao.com/yws/res/26397/74C3A3A5479D4F0AA5F4FE3706BEE26B)
- git status 看看什么情况，修改了a.txt，但是此时a.txt的修改还没有拉到自己的本地文件中，提示 git checkout
- git chectout a.txt , 新输入的不见了，回退成功。

### 回退到任何一个版本号
- git reflog，显示从建立本地仓库开始所有的提交历史
![image](https://note.youdao.com/yws/res/26421/17F8AC1CCD00419681B68205E5C3B94D)
- 找到自己的commit 的记录，最前面的就是版本号，注意这里现在时缩写的，复制，输入 git reset --hard 版本号 ，回退到当时的commit，加入--hard 不需要后面的checkout。
![image](https://note.youdao.com/yws/res/26429/5516AFF2BF6C4357A3B87E72EF83447C)
- git add .    git  commit -m "back to..."
- git status  看看情况
## 工作区，暂存区，版本库
![image](https://note.youdao.com/yws/res/26438/9647C9F83A3D4CEA9BEA1C9D14AC5D8C)
[git工作区、暂存区、版本库之间的关系](https://www.cnblogs.com/lianghe01/p/5846525.html)

- **工作区**就是自己的本地电脑的文件目录，开发的文件夹，通过add 命令将其推到版本库
- 在**版本库**中有stage/index（暂存区） 和 master两个
    - 在index中确认无误后使用commit，把暂存区中的内容放到最最终的版本库

### 例子
- 可以看到add 之后是在暂存器之中，此时需要二次的commit
![image](https://note.youdao.com/yws/res/26467/7796EAEFF7974C74BA2E60CB3424E2AE)
- echo "we are a good boy" >c.txt 在c.txt中add new line
- 三条命令
    - git diff 查看工作区和暂存区的差别
    - git diff --cached 比较的是暂存区和版本库的差别
    - git diff HEAD可以查看工作区和版本库的差别。
- 从图中可以看出，工作区与暂存区、工作区与版本库都有变化，而版本库与暂存区并没有变化，**就是只在工作区变化了，暂存区和版本库不变**
![image](https://note.youdao.com/yws/res/26477/C67EF793634148D68DDE1FAC9F754936)
- 执行git add c.txt 后，再使用这三个命令，表明工作区与暂存区已经同步，暂存区与版本库、版本库与工作区没有同步。
![image](https://note.youdao.com/yws/res/26496/E82E708CF13245C19A19EB4514B0E461)
- 执行 git commit -m "new file c.txt"后，执行三个命令发现没有变化。就是说三个区全部同步了。
![image](https://note.youdao.com/yws/res/26504/E4658D3DD1BF4D9691CF4CD324C8CCA3)

## 如何恢复删除操作
### 删除了还没有add到暂存区
- 新建一个f.txt ;touch f.txt
- git add f.txt -> git commit -m "new file f.txt" -> rm f.txt -> git status查看状态
![image](https://note.youdao.com/yws/res/26516/DE341E1286A145E6A55373CB14B1DA69)
- 看提示，git checkout f.txt就可以

### 提交了删除操作
- git add f.txt -> git commit -m "new file f.txt" -> rm f.txt -> git add f.txt->git commit -m "delete f.txt for sure" -> git log f.txt，可以看到查不到了
![image](https://note.youdao.com/yws/res/26530/3B18AB79F21E4E41ABE62702EEB6E04F)
- git reflog 看所有的log，然后找到 add f.txt 对应的版本号
- git reset --hard 版本号 -> git add . -> git commit -m "welcome back f.txt",恢复删除的文件
![image](https://note.youdao.com/yws/res/26543/0F362FA5032F4E55A69D6CB902A3F456)

## git 的分支和tag
### 分支
 - git branch: 列出当前版本库中所有的分支
 - git checkout -b branch_01: 复合语句，新建分支+跳到当前分支。新建分支后，新建立的所有的东西都和现在所在的master分支是一致的，可以说是浅拷贝。
![image](https://note.youdao.com/yws/res/26573/6C34B95B2F9D4E53A2A342B484B4F1DD)
- git checkout master: 切换到一个分支，注意这里没有 -b
- 新建一个 g-branch01.txt，增加一行，就在branch01上多了一个文件
![image](https://note.youdao.com/yws/res/26581/D2C6A9F753C740C1BD882F59BF015220)
- 看看两个分支下的不同，可以看到master中式没有g-branch01.txt的，因为是创建在branch_01下的，**不同的分支之间是相互不干扰的**
> 在branch_01下的本地仓库

![image](https://note.youdao.com/yws/res/26588/69D2F6008CAE4F5881AC9C1709D1D2EF)
> 在master下的本地仓库

![image](https://note.youdao.com/yws/res/26593/A619FCE082DB43679A1B42DC719DEAC2)
- git merge branch_01：将branch_01合并到master上，因为可能独立开发完了，将代码合并，这里需要**注意合并之前需要将分支切换到你要操作的分支，在这里切换到master上**，这里可能会出现冲突。**merge操作不需要commit什么的 **
![image](https://note.youdao.com/yws/res/26613/A8AD0F61E96F454EA708FB6C8AF50750)

### tag
当需要发布版本的时候，不可能直接将master发布上去，需要打一个tag里程碑一样的,实现版本迭代
- git tag v1.1: 注意需要切换到要操作的分支
![image](https://note.youdao.com/yws/res/26629/271FAC252F384E6A921AE04E1ECB466F)





## 使用GitHub保存代码
### 将本地仓库上传github中心
[使用git将项目上传到github（最简单方法）](http://www.cnblogs.com/cxk1995/p/5800196.html)

[使用git保存管理代码](https://www.cnblogs.com/sunny0/p/7909609.html)

[如何使用git把本地代码上传（更新）到github上](https://baijiahao.baidu.com/s?id=1619544681032320225&wfr=spider&for=pc)

- touch a.html -> git add . -> git commit -m "add a.html";可以看到建议使用 git push 推到github上
![image](https://note.youdao.com/yws/res/26633/DFD5FBAFE386402AA1AAE0766B7EBFAD)
- git push ;
![image](https://note.youdao.com/yws/res/26642/2A2EE0D29FFC4657BF2227C32FD3636C)

> github 上就有了

![image](https://note.youdao.com/yws/res/26644/466B5C15B4DF4329933616CE3A54F165)

### 将github中心同步到本地
- 在github上新建一个c.html
- git pull; ls看一下
![image](https://note.youdao.com/yws/res/26659/CFF692F3C7C74A80BB5D8D6692EBF609)

> github 官网

![image](https://note.youdao.com/yws/res/26850/C88CAA4F8FDC4924B413DE517C3BEDF0)

## github权限和分支
如何控制giehub访问权限和建立本地github分支
### github权限
setting -> Interaction limits -> push access -> add collaborator 可以添加一个变更代码权限的合作者

![image](https://note.youdao.com/yws/res/26676/A0C3559A6D554A49BBED4A0E63BEC3F5)

### 在本地创建github分支






# 问题
## 已经push到Github后要撤销到某个版本号
- git revert 版本号
![image](https://note.youdao.com/yws/res/28365/0A0B877BA2B342DEA28DD0C14D014C78)

[git push 操作代码回退](http://www.cnblogs.com/linchv587/p/9075478.html)

但是这样做了之后本地仓库了的代码不见了？？？？

有git clone了一下，准备改了之后再次提交，遇到了很多问题

###  不能直接push，要先pull同步一下
拒绝合并，默认创建了 README.md文件。

![image](https://note.youdao.com/yws/res/28381/900CF42D0BD34D3BB2129E08E24B1F06)

但是pull的时候，也会报错，让本地库与远程库同步。

但是不能直接 git pull origin master，因为我是在本地创建的仓库不是 git clone的
[Git使用 - 本地项目与 GitHub 远程仓库同步](https://blog.csdn.net/ithanmang/article/details/80869923)

![image](https://note.youdao.com/yws/res/28392/760F305F5063484D80D5E344D5C5ECD1)

需要git pull origin master --allow-unrelated-histories

![image](https://note.youdao.com/yws/res/28398/DC989E10465B4B1C89D293AFCB4C94D4)

然后push

![image](https://note.youdao.com/yws/res/28400/EA8BC7A9A68F4BD7995F5D1CBCBBAB9D)

## 删除GitHub仓库的中某个文件夹

[删除GitHub仓库的中某个文件夹](https://blog.csdn.net/smss007/article/details/80974219)

> 不行的话就进行clone

- git remote add https://github.com/xctspring/JavaBasic.git
- git pull origin master
- git rm -r --cached "Day07_Custom class" "Day08_Class Variable"
- git commit -m "remvove several folders"
- git push -u origin master

![image](https://note.youdao.com/yws/res/28593/382CD6DEC00D4C74BBA2666989D6A0C2)



