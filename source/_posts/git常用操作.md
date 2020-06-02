---
title: git常用操作
date: 2019-06-02 18:54:53
tags:
  - git
---
记录一下常用的git命令

<!--more-->
```
git init
```
创建一个本地仓库
```
git clone [git-url] [user-name]
```
克隆一个远程仓库到本地,并且重命名 user-name可省,默认为远程仓库名
```
git clone -b [git-branch] [git-url] [user-name]
```
从指定分支分支克隆远程仓库到本地,并且重命名 user-name可省
```
git remote [git-params] 
```
列出远程仓库别名,默认会有(fetch/push)两条数据,这是因为git允许你为每个仓库添加不同的推送和获取连接,默认git-params缺省,此时只显示别名
git-params 
-v 列出远程仓库别名和地址
```
git remote add [git-alias] [git-url]
```
添加一个远程仓库, git-alias通常为origin 
```
git remote rm [git-alias]
```
删除一个远程仓库
```
git add . / git add [file]
```
添加文件到缓存区, .表示全部文件 file表示指定文件
```
git reset HEAD [add-file]
```
用于git add后撤销操作, add-flie省略则对上次add操作全部撤销
```
git status [git-params]
```
查看工作目录和缓存区的状态
git-params
-s 简洁方式输出,只提示文件和状态
```
git commit -m ''
```
提交至本地仓库,并备注
```
git reset [git-params] [commit-id]
```
撤销commit操作, git log 获取提交信息, 得到commit-id
git-params:
--mixed(默认) 撤销git commit, 撤销git add, 保留编辑器改动代码
--soft 撤销git commit, 不撤销git add, 保留编辑器改动代码
--hard 撤销git commit, 撤销git add, 删除编辑器改动代码
```
git reset --mixed HEAD^/HEAD~1
```
也是撤销操作 HEAD^(HEAD~1)表示上一版
```
git pull [git-alias] [online-git-branch]:[local-git-branch]
```
将远程仓库git-alias下的online-git-branch远程分支拉取下去与local-git-branch本地分支合并, local-git-branch可省略,此时默认与当前分支合并

```
git push [git-alias] [local-git-branch] [online-git-branch]
```
将local-git-branch本地分支上的内容推送到git-alias远程仓库上的online-git-branch分支
online-git-branch 可省,则表示将本地分支推送到与之存在追踪关系的远程分支(通常两者同名),如果该远程分支不存在,则会被新建
```
git branch [branch-name]
```
创建本地分支
```
git checkout [branch-name]
```
切换到指定分支
```
git checkout -b [branch-name]
```
创建并切换到指定分支
```
git branch -a
```
查看所有分支,以及当前所处分支, -a省略则查看本地分支
```
git branch -D [branch-name]
```
删除本地分支,(处于该分支时不能删除)
```
git fetch [git-alias] [git-branch]
```
从远程仓库git-alias拉取git-branch到本地仓库, git-branch省略,则拉取所有分支最新信息,包含新建分支
```
git merge [git-alias/git-branch]
```
把git-alias/git-branch分支上的内容合并到当前分支上(分支写法有些特殊,同时包含远程仓库名/远程分支)
