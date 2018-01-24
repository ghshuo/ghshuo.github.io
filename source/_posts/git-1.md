---
title: 一台电脑上使用多个git账号
date: 2018-01-17 20:10:50
categories: "GIT" #文章分类目录
tags: GIT #文章标签 可以省略
description: #你對本頁的描述 可以省略
---
![git操作总结](/images/git.jpg)
<!-- more -->
这里以work和github两个账户为例：
### 取消全局变量
```js
git config --global --unset user.name  #取消全局设置
git config --global --unset user.email #取消全局设置
git config -l #查看当前目录的git config
```
再分别去不同的项目目录中，设置这个目录中项目对应的账号
```js
git config user.name "newname"
git config user.email "newemail"
```
### 生成两个秘钥
```js
$ ssh-keygen -t rsa -C "work@github.com"
$ ssh-keygen -t rsa -C "github@github.com"
```
在C:\Users\hsgeng\.ssh文件下会生成四个文件
```js
id_rsa_work
id_rsa_work.pub
id_rsa_git
id_rsa_git.pub
```
![git中ssh生成秘钥](/images/git_ssh.png)
  

### 设置ssh config
```js
Host work  
    HostName git.mobicloud.com.cn
    IdentityFile /c/Users/hsgeng/.ssh/id_rsa_work
    PreferredAuthentications publickey
    User work

Host git 
    HostName github.com
    IdentityFile /c/Users/hsgeng/.ssh/id_rsa_git
    PreferredAuthentications publickey
    User git
```
### 最后

分别把秘钥添加到对应的账号上面就行了

如果另外一台电脑上面也需要用这个这个账号、只需要把生成的ssh下的文件复制到另一台电脑就行。

如果有那个地方写错，请大家指出来，感谢大家！