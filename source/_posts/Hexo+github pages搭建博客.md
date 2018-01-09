---
title: Hexo + github pages搭建博客
date: 2017-11-10 22:06:40
categories: "Hexo" #文章分类目录
tags: Hexo #文章标签 可以省略
description: #你對本頁的描述 可以省略
---

![2017年年度总结](/images/hexo-github.jpg)
<!-- more -->

### 前言
  在技术成长的过程中，肯定会遇到各种各样的问题，为了方便节约重复问题的时间，同时也可以更方便的和很多朋友共同学习。程序猿这个道路上，只有不断的学习才能进步。
今天用Hexo + github pages搭建个人技术博客。

### node.js安装
  [node.js官方文档](https://nodejs.org/en/)
  参考这个(nodejs官方文档)



### GitHub Pages
#### 定义
    1.GitHub Pages是一个静态站点托管服务， 是通过我们网站托管和发布的公开网页。
    2.github Pages学习成本低，相比其他搭建方式而已谈，不需要太多的服务器基础。
    3.轻量级的博客系统，没有麻烦的配置
    4.使用标记语言，比如Markdown

#### git安装
 [git官网](https://git-scm.com/)

  注册github
这里就不多说了，点击git注册地址[这里](https://github.com/join?source=header)
 创建仓库
      - 先点击 new repository 创建仓库
      - 填写仓库名称    github用户名称.github.io
      - 确定创建
  如下图：
![创建github](/images/github.png)

#### 添加秘钥
用git生成秘钥 ssh-keygen -t rsa -C "Github的注册邮箱地址"

id_rsa和id_rsa.pub 生成这个两个文件 打开id_rsa.pub文把秘钥填写到github上[这里](https://github.com/settings/keys)

### hexo
#### 定义
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

[hexo官方文档](https://github.com/settings/keys)

#### 安装hexo

```
    $ npm install -g hexo-cli
    $ hexo init hexo 初始化文件夹
    $ cd hexo  切换到该路径
    $ npm install 安装hexo扩展插件
``` 
  #### 本地服务器查看
``` 
    $ npm install hexo-server --save
    $ hexo generate  生成静态页面
    $ hexo server
    $ hexo server -i 192.240.1.1  自定义iP运行
``` 
安装完成后，输入以下命令以启动服务器，您的网站会在 http://localhost:4000 下启动。在服务器启动期间，Hexo 会监视文件变动并自动更新，您无须重启服务器。

  #### 线上部署
      $ hexo generate --deploy
      $ hexo generate  生成静态页面
      npm install hexo-deployer-git --save
      $ hexo deploy 部署到github线上

输入http://ghshuo.github.io 进行查看

hexo generate：
      生成静态文件。将我们的数据和界面相结合生成静态文件的过程。
      会遍历主题文件中的 source 文件夹（js、css、img 等静态资源），
      然后建立索引，
      然后根据索引生成 pubild 文件夹中，
      此时的 publid 文    件是由 html、 js、css、img 建立的纯静态文件
      可以通过 index.html 作为入口访问你的博客。

hexo deploy：

      部署文件。部署主要是根据在 _config.yml 中配置的 git 仓库或者 coding 的地址，
      将 public 文件上传至 github 或者 coding 中。
      然后再根据上面的 github 提供的 pages 服务呈现出页面。
      当然你也可以直接将你生成的 public 文件上传至你自己的服务器上。

 hexo 常用命令
      hexo new"postName" #新建文章
      hexo new page"pageName" #新建页面
      hexo clean # 删除静态页面至public目录
      hexo generate #生成静态页面至public目录
      hexo server #开启预览访问端口
      hexo deploy #将.deploy目录部署到GitHub
      hexo help # 查看帮助
      hexo version #查看Hexo的版本



### 总结：
我感觉hexo就是一个快速、简洁且高效的博客框架，对markdown文件的重新渲染引擎，生成静态网页，并且和GitHub Pages静态站点托管服务，通过我们网站托管和发布的公开网页。



### 参考
link [hexo原理浅析](https://segmentfault.com/a/1190000008784436)
link [hexo原理](https://juejin.im/post/598eeaff5188257d592e55bb#heading-1)