---
title: GIT安装配置
date: 2018-01-16 20:10:50
categories: "GIT" #文章分类目录
tags: GIT #文章标签 可以省略
description: #你對本頁的描述 可以省略
---
![git操作总结](/images/git.jpg)
<!-- more -->

### git是什么
Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。  Git的读音为/gɪt/。
Git是一个开源的分布式版本控制系统，可以有效、高速的处理从很小到非常大的项目版本管理。  Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
### 安装
Windows上使用Git，可以从[git官网下载](https://git-scm.com/)安装程序

如果下载成功之后默认安装即可，然后再在菜单中找到Git --> Git Bash 打开出现下图命令的窗口说明已经安装成功。


![git安装成功显示](/images/gitaz.png)
### git配置和使用
#### 设置Git个人信息
```js
$ git config --global user.name "hsgeng"  名字
$ git config --global user.email "648715552@qq.com"  邮箱
``` 
#### 生成密钥
```js
$ ssh-keygen -t rsa -C "humingx@yeah.net"
``` 
生成之后在C:\Users\hsgeng\.ssh  下生成id_rsa和id_rsa.pub两个文件

#### 添加秘钥
这里以github为例
打开id_rsa.pub文把秘钥填写到github上[这里](https://github.com/settings/keys)
#### 测试是否正常
ssh -T git@github.com
会显示
```js
The authenticity of host 'github.com (207.97.227.239)' can't be established. RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. Are you sure you want to continue connecting (yes/no)?
输入yes后，显示出下列信息表示连接成功
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
``` 
### 创建代码库
#### 创建本地代码库
```js
mkdir ~/github  创建目录
cd /github 更换到当前目录下
git init 初始化改文件
``` 
#### 克隆代码
```js
git clone https:// 克隆github项目的地址
``` 
克隆指定分支
```js
git clone -b blog（分支名称） https://克隆github项目的地址
``` 
### 提交代码
每次提交之前都需要push下  更新下代码
```js
git pull  // 更新
git add .  //添加
git commit -a"更改内容"  //commit 标记
git push origin master  // master分支名称    
``` 
