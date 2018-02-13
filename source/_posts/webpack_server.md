---
title: webpack执行npm run server报错解决方法
date: 2018-02-13 11:59:56
categories: "webpack" #文章分类目录
tags: webpack #文章标签 可以省略
---

### 运行命令
运行npm run server 
### 出现错误
```js
> webpack-demo@1.0.0 server F:\webpack-demo
> webpack-dev-server

events.js:160
      throw er; // Unhandled 'error' event
      ^

Error: listen EADDRNOTAVAIL 172.16.0.112:1919
    at Object.exports._errnoException (util.js:1018:11)
    at exports._exceptionWithHostPort (util.js:1041:20)
    at Server._listen2 (net.js:1245:19)
    at listen (net.js:1294:10)
    at net.js:1404:9
    at _combinedTickCallback (internal/process/next_tick.js:83:11)
    at process._tickCallback (internal/process/next_tick.js:104:9)
    at Module.runMain (module.js:606:11)
    at run (bootstrap_node.js:389:7)
    at startup (bootstrap_node.js:149:9)
    at bootstrap_node.js:504:3
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! webpack-demo@1.0.0 server: `webpack-dev-server`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the webpack-demo@1.0.0 server script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\hsgeng\AppData\Roaming\npm-cache\_logs\2018-02-13T03_41_46_926Z-debug.log
```

### 解决方法 
查看端口是否被占用、ip地址是否正确。 
被占用的端口可以结束进程或者更换一个端口号
ip不正确的修改下ip地址 

