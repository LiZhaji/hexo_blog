---
title: Git
date: 2020-02-15 00:12:47
tags:
categories: Git
---
# 文档

[已学习链接](https://www.liaoxuefeng.com/wiki/896043488029600)

[官网文档](https://git-scm.com/docs)

[分支游戏](https://learngitbranching.js.org/?NODEMO)
# 目的

了解git的内部原理，各种命令干了什么。
# 背景

1991年诞生了Linux，由全球各地的人合作产生，从最初的人工合并diff代码，到BitKeeper供其免费使用，最后因为微软人试图破解BitKeeper协议被发现，2002年促使了Git的诞生。
- 集成式版本控制系统：每次干活之前把最新版本代码从服务器取过来，干完活后上传新版本到服务器。全都依赖服务器，需要联网。如CVS/SVN/VSS
- 分布式版本控制系统：每个人电脑都有一个完整的版本库。不需要联网。如Git/BitKeeper/Mercurial/Bazaar
# 使用须知

- HEAD是指向当前分支下当前版本的指针，HEAD^表示上一个版本，HEAD~100往上一百个版本
- 仓库分为工作区和暂存区
- 同一个本地库可以关联多个不同的远程库，如GitHub 码云等。需要用不同的名称来标识。git remote add origin xxx 默认名称是origin
# 常用命令

git status 查看当前版本库状态
git diff 查看difference
git log 由近到远显示提交日志
--pretty=oneline 查看主要信息
--graph 查看分支合并图
git reflog 记录历史命令
git checkout 切换分支/撤回更改
git reset 版本退回/撤回add操作
git remote 查看远程仓库
-v 查看远程仓库信息
rm origin 删除已关联的远程仓库origin
git config --scope  自定义git，如更改样式/自定义快捷命令 scope默认是当前仓库
仓库在保存.git/config 下，用户在.gitconfig下
alias.newCommand 'origin command' 自定义快捷命令
# 常见场景

- 获取ssh
  - 位于.ssh目录，id_rsa 私钥，id_rsa.pub 公钥
  - ssh-keygen -t rsa -C "your email" 生成ssh
  - Git支持SSH协议，目的是确保该提交的确是本人提交
- 新建代码库
case1:
  - 新建空库
  - 到代码文件 git init
  - git add .
  - git commit -m "update"
  - git remote add origin xxxx // origin是默认的远程仓库的名字
  - git push
  - 根据提示  
case2:
  - 新建空库
  - git clone xxxx
  - 把代码拖进去
- 新建并合并代码
  - 创建newBranch分支的实质：增加一个newBranch的指针，指向旧分支相同的提交，将HEAD指向dev分支
  - git branch 查看分支情况 -d branch 删除分支
  - git branch newBranch 创建新分支
  - git checkout branchName 切换分支
  - git checckout -b newBranch   Switched to a new branch 'newBranch' 新建并切换到newBranch分支，从哪个分支切过去就会copy那个分支上的所有代码
  - git switch
  - git merge aBranch 合并aBranch分支上的代码
    - Fast-forward 快进模式，将master指向aBranch的当前提交，删除分支后会丢掉分支信息，看不出合并过
    - --no-ff 普通模式，关闭快进模式，会生成一个commit
- 文件修改操作
  - git checkout -- file 退回工作区的修改，更改至最近一次add或commit后的状态  === vscode Discard Changes操作
  - git reset HEAD file 退回暂存区stage里的修改，撤销add 操作 === vscode Unstage Changes操作
  - git rm file 删除文件
- 回退版本
  - git reset --hard commit_id/HEAD^/HEAD~n  commit_id可以只取前几位
  - git checkout commit_id/HEAD^/HEAD~n
  - git log 查看提交历史，确定回退到哪个版本
  - git reflog 查看命令历史，确定回到未来的版本
- 修改bug，临时保存工作区

  - git stash 临时存储当前保存，使工作区保持干净。一般用于有代码变动未提交时想要切换分支
  - list 查看临时存储列表
  - pop 恢复现场并删除存储列表内容
  - apply ?stash@{0} 恢复某一现场，不从临时存储列表删除
  - git cherry-pick commit_id 复制一个commit到本分支，避免重复劳动
# Linux

mkdir 创建文件夹

echo 创建文件