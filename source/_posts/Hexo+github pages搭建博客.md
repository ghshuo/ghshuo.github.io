---
title: Hexo + github pages搭建博客
date: 2017-11-10 22:06:40
categories: "Hexo" #文章分类目录
tags: Hexo #文章标签 可以省略
description: #你對本頁的描述 可以省略
---
#### GitHub Pages

	1.GitHub Pages是一个静态站点托管服务， 是通过我们网站托管和发布的公开网页。
	2.github Pages学习成本低，相比其他搭建方式而已谈，不需要太多的服务器基础。
	3.轻量级的博客系统，没有麻烦的配置
	4.使用标记语言，比如Markdown
#### hexo
1. 定义 官网给出的
	  ：Hexo 是一个快速、简洁且高效的博客框架。
	  Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
2. Hexo 的工作原理
hexo g：

			生成静态文件。将我们的数据和界面相结合生成静态文件的过程。
			会遍历主题文件中的 source 文件夹（js、css、img 等静态资源），
			然后建立索引，
			然后根据索引生成 pubild 文件夹中，
			此时的 publid 文		件是由 html、 js、css、img 建立的纯静态文件
			可以通过 index.html 作为入口访问你的博客。
hexo d：

			部署文件。部署主要是根据在 _config.yml 中配置的 git 仓库或者 coding 的地址，
			将 public 文件上传至 github 或者 coding 中。
			然后再根据上面的 github 提供的 pages 服务呈现出页面。
			当然你也可以直接将你生成的 public 文件上传至你自己的服务器上。
3. hexo的模板引擎

			模板引擎的作用，就是将界面与数据分离。
			最简单的原理是将模板内容中指定的地方替换成数据，实现业务代码与逻辑代码分离。
总结：
		
    我感觉hexo就是一个快速、简洁且高效的博客框架，对markdown文件的重新渲染引擎，生成静态网页，并且和GitHub Pages静态站点托管服务，通过我们网站托管和发布的公开网页。
#### 参考

link [hexo原理浅析](https://segmentfault.com/a/1190000008784436)
link [hexo原理](https://juejin.im/post/598eeaff5188257d592e55bb#heading-1)