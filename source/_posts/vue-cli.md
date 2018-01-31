---
title: Vue-cli脚手架安装及结构介绍
date: 2018-01-21 20:19:56
categories: "vue" #文章分类目录
tags: Vue-cli #文章标签 可以省略
---
![vue-cli脚手架](/images/vueCli.png)
<!-- more -->
### 定义
 Vue-cli是快速构建这个单页应用的脚手架
### 环境搭建
#### 安装node.js
从这个[node.js官方文档](https://nodejs.org/en/)中下载安装，安装过程很简单
安装完成之后打开命令行工具 输入： 
```js
node -v
```
出现相应的版本号说明安装成功
![node.js](/images/nodeJs.png)

#### npm
npm其实是Node.js的包管理工具（package manager）

因为我们在Node.js上开发时，会用到很多别人写的JavaScript代码。如果我们要使用别人写的某个包，每次都根据名称搜索一下官方网站，下载代码，解压，再使用，非常繁琐。于是一个集中管理的工具应运而生：大家都把自己开发的模块打包后放到npm官网上，如果要使用，直接通过npm安装就可以直接用，不用管代码存在哪，应该从哪下载

输入命令
```js
npm install -g npm --registry= https://registry.npm.taobao.org
```
出现相应的版本号说明安装成功
![npm](/images/npm.png)

### webpack安装
全局安装
```js
npm install webpack -g
```

### vue-cli脚手架
```js
npm install vue-cli -g
```
### 构建项目
#### 初始化
```html	
$ vue init <template-name> <project-name>
```
#### 创建项目 
 用vue-cli脚手架新建一个项目
```js
$ vue init webpack vuecli ----- 这个是那个安装vue脚手架的命令
? Project name (vuecli)  ----- 项目名称
? Project name vuecli
? Project description (A Vue.js project) -----------项目描述
? Project description A Vue.js project
? Author (hsgeng <648715552@qq.com>) hsgeng
? Author hsgeng   --------- 项目创建者
? Vue build (Use arrow keys)
? Vue build standalone
? Install vue-router? (Y/n) -------- 是否安装路由
? Install vue-router? Yes
? Use ESLint to lint your code? (Y/n) n ------是否启用eslint检测规则
? Use ESLint to lint your code? No
? Setup unit tests with Karma + Mocha? (Y/n)  ------是否安装单元测试
? Setup unit tests with Karma + Mocha? n
? Setup e2e tests with Nightwatch? (Y/n) ---------是否安装模拟测试
? Setup e2e tests with Nightwatch? n
vue-cli · Generated "vuecli".
To get started:  
cd vuecli  
npm install 
npm run dev    
```
#### 找到项目目录
```js
cd  vuecli
```
#### 安装项目依赖
```js
npm install 
```
#### 运行
```
npm run dev 
```
### 项目结构
```js
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关
|   |-- dev-server.js                // 构建本地服务器
|   |-- utils.js                     // 构建工具相关
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|   |-- test.env.js                  // 测试环境变量
|-- src                              // 源码目录
|   |-- components                     // vue公共组件
|   |-- store                          // vuex的状态管理
|   |-- App.vue                        // 页面入口文件
|   |-- main.js                        // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|   |-- data                           // 群聊分析得到的数据用于数据可视化
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- README.md                        // 项目说明
|-- favicon.ico 
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息
```
#### 参考
http://jspang.com/2017/04/10/vue-cli/