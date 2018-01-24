---
title: GIT常用命令总结
date: 2018-01-17 22:10:50
categories: "GIT" #文章分类目录
tags: GIT #文章标签 可以省略
description: #你對本頁的描述 可以省略
---
### git基本命令
```js
git add 将想要快照的内容写入缓存区
git status -s "AM" 状态的意思是，这个文件在我们将它添加到缓存之后又有改动
git commit -m '第一次版本提交' -m选项添加备注信息
git clone url 使用 git clone 拷贝一个 Git 仓库到本地
git diff 查看执行 git status 的结果的详细信息
　　尚未缓存的改动：git diff
　　查看已缓存的改动： git diff --cached
　　查看已缓存的与未缓存的所有改动：git diff HEAD
　　显示摘要而非整个 diff：git diff --stat
git commit -a 跳过git add 提交缓存的流程 
git reset HEAD 用于取消已缓存的内容
git rm file 
　　git rm 会将条目从缓存区中移除。这与 git reset HEAD 将条目取消缓存是有区别的。
　　"取消缓存"的意思就是将缓存区恢复为我们做出修改之前的样子。
　　默认情况下，git rm file 会将文件从缓存区和你的硬盘中（工作目录）删除。
git mv 重命名磁盘上的文件 如 git mv README README.md

git push -u origin master 提交代码
```

### git 分支管理

创建分支命令 git branch (branchname) 列出分支 git branch
切换分支命令 git checkout (branchname)
合并分支 git merge (branchname)
创建新分支并立即切换到该分支下 git checkout -b (branchname)
删除分支命令 git branch -d (branchname)
ps:状态 uu 表示冲突未解决 可以用 git add 要告诉 Git 文件冲突已经解决
### 更改提交的操作
```
　　git reset //回溯历史版本
　　git reset --hrad //回溯到指定状态，只要提供目标时间点的哈希值
```
### 修改提交信息 git commit --amend
　　压缩历史 git rebase -i 错字漏字等失误称作typo
　　根据以前的步骤在GitHub上创建仓库，应于本地的仓库名相同 GitHub上面创建的仓库的路径为git@github.com: 用户名/仓库名.git
　　git remote add eee git@github.com: 用户名/仓库名.git //添加远程仓库，并将git@github.com: 用户名/仓库名.git远程仓库的名称改为eee
　　git push -u eee master //推送至远程仓库 master分支下 -u 参数可以在推送的同时，将eee仓库的master分支设置为本地仓库的当前分
　　支的的upstream（上游）。添加这个参数，将来运行git pull命令从远程仓库获取内容时，本地仓库的这个分支就可以直接从eee的master
分支中获取内容
　　git checkout -b feature d eee/feature d //获取远程的feature d分支到本地仓库，-b参数后面是本地仓库中新建的仓库的名称
　　git pull eee feature d //将本地的feature d分支更新为最新状态
　　在GitHub上面查看两个分支之间的差别，只需要在地址栏中输入http://github.com/用户名/仓库名/分支1...分支2
### 查看日志版本
```js
git log 命令列出历史提交记录
git log --oneline 查看历史记录的简洁的版本
git log --oneline --graph 查看历史中什么时候出现了分支、合并
```
### 标签
为软件发布创建标签是推荐的。这个概念早已存在，在 SVN 中也有。你可以执行如下命令创建一个叫做 1.0.0 的标签：
git tag 1.0.0 1b2e1d63ff
1b2e1d63ff 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：
git log
你也可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性

### 提取远程仓库代码
```js
git fetch　　从远程仓库下载新分支与数据
git pull　　从远端仓库提取数据并尝试合并到当前分支
```
### 
参考 https://www.cnblogs.com/hexiaobao/p/8134829.html
https://www.cnblogs.com/lhxiaosoft/p/6400812.html
