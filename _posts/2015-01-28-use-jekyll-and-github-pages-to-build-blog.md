---
layout: post
title: 使用Jekyll和Github Pages构建个人博客
---

这篇Blog主要就是介绍我是如何使用Jekyll和Github pages快速得搭建了我的这个个人博客。

有几个很重要的前提决定了我采用这样的方案来搭建博客，而不是安装一个wordpress或者其他的博客系统。

* 我要有自己的域名，比如：www.bigbing.net
* 我不想自己架设服务器或者租借空间，我想要免费的东西
* 作为一个开发者，我想要用markdonw来写东西
* 我写好的东西，能够有版本控制，而且备份到服务器上
* 我想要自己控制网站的样式和功能

基于这样的一些前提，我很容易的就发现了github pages这个功能，特别适合开发者使用，因为大部分开发者都已经有了github的个人帐号，而github pages支持使用Jekyll来快速生成网站页面和blog。

接下来我不会详细说明具体的步骤，我主要提供一些资源的连接，通过这些资源任何人都可以快速得搭建出自己的个人网站。

Github pages的官方[快速上手指南](https://pages.github.com/)

Github官方提供了一个GitHub Pages Gem, 帮助你快速配置好jekyll的开发运行环境，保持跟github服务器上运行的环境一致，具体的[使用文档](https://help.github.com/articles/using-jekyll-with-pages/)


下面这是一篇很长，但是很实用，系统的介绍文章，里面也提供了很多相关资源的连接：[Link](http://www.smashingmagazine.com/2014/08/01/build-blog-jekyll-github-pages/)


我搭建的这个blog主要使用了这个叫[Lanyon的theme](https://github.com/poole/lanyon),另外还有一个也不错的是叫[hyde](https://github.com/poole/hyde),这两个开源的theme都是基于[Poole](https://github.com/poole/poole)。
