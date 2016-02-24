---
layout: post
title: Rails4新项目之CSS框架的选择：Bourbon
---

![](/images/Bing_700.JPG)

我在之前的项目中使用过[Bootstrap](http://getbootstrap.com/)和[Foundation](http://foundation.zurb.com/)，确实非常方便使用，把相关的gem放到项目中之后，然后再到网站上找到一些HTML和CSS的例子，基本能够满足需求，但是要说到对这两个框架有多了解，还真是谈不上，这两个都是大而全的东西，在Rails中基本上都是一股脑全部引入，然后就只管用，确实省心。

但是时间长了就发现，用这两个框架写出来的页面代码还是非常啰嗦，易读性和可维护性随着项目的变大越来越差。

之前还有一个项目用到了[Compass](http://compass-style.org/)，`Compass`中也是包含了很多内容，我个人一直比较喜欢简单，轻量的东西，所以看到这种庞大的库就有点害怕。

然后我就想有没有其他选择呢？后来，在一个开源项目[Hours](https://github.com/DefactoSoftware/Hours)中就发现了[Bourbon](http://bourbon.io/)，然后又是[Neat](http://neat.bourbon.io/)，还有[Bitters](http://bitters.bourbon.io/)和[Refills](http://refills.bourbon.io/)，一开始一下看到四个东西，感觉摸不清思路，都不知道是干什么的。后来，读了几篇文章后，就清楚了`Bourbon`整个技术栈的理念。就是把像`Bootstrap`这种大而全的框架，清楚地拆分成四个小的工具库，然后可以组合在一起使用，也可以根据需要来选择使用。

下面简单介绍一下四个工具：

* [Bourbon](http://bourbon.io/): 简单的轻量级的`Sass Maxin`库
* [Neat](http://neat.bourbon.io/): 一个轻量级的语义化的页面网格框架
* [Bitters](http://bitters.bourbon.io/): 脚手架(scaffold)样式,变量的定义，方便快速开始Bourbon的项目
* [Refills](http://refills.bourbon.io/): 基于以上三个的一些实用的组件库


另外看到`Bourbon`是[thoughtbot](https://thoughtbot.com/)出品的，我相信这些工具一定在实际项目中得到了验证。我一直以来都很喜欢`thoughtbot`开发一些gems，非常实用，而且轻量，比如`clearance`等。

其中下面这两篇文章说服了我：

[5 Reasons We Chose Bourbon/Neat Over Foundation or Bootstrap](http://www.amplificommerce.com/5-reasons-we-chose-bourbonneat-over-foundation-or-bootstrap/)

[Why I prefer Bourbon over Bootstrap](http://federicoramirez.name/why-i-prefer-bourbon-over-bootstrap/)


最吸引我得就是使用`Bourbon`和`Neat`，可以写出非常易读，易懂的`语义化的HTML`

特别是当我们使用`Haml`或者`Slim`这样的模板语言后，在模板中如果都是一些`语义化的CSS class`，代码看起来就非常直观。

比如：
用Bootstrap会这样写(如果再加上一些响应式的样式就更加冗长)：
{% highlight html %}
.container
  .row
    .col-md-6 /* some content */
    .col-md-6 /* some content */
{% endhighlight %}

使用Borbon可能会是这样，非常清晰，直观：
{% highlight html %}
.news
  .hot-news
  .latest-news
{% endhighlight %}

接下来简单说一下我是如何在新的Rails项目中使用的。

####1. 基本安装:
这里没有什么特别需要说明的，大家只要按照官方github上的文档说明，很容易搞定。

####2. 使用normalize.css
在Bootstrap中默认是集成了`normalize.css`，所以我们不用单独引入，在Bourbon栈中默认是没有的，需要我们单独引用.这里使用normalize-rails这个gem就可以了。

> gem "normalize-rails"

####3. 使用`autoprefixer`
Bourbon中有一大块功能是关于`vendor prefixes`的。需要开发者自己主动地去使用封装好的一些Sass的mixin，这个对于我一个后端出身的号称全栈的开发者要求有些高了。
同时autoprefixer的处理方式就是非常的傻瓜式，开发者自己不要管vendor prefixes，尽管写标准的CSS，剩下的autoprefixer自动搞定。
Bourbon的开发者也同意这一点，所以在`Bourbon5.0`中，关于vendor prefixes的部分也将会删除。
所以，我们只需要使用gem "autoprefixer-rails"就可以了。

> gem "autoprefixer-rails"

如果想了解和深入学习Bourbon相关的技术，可以到官方网站上查看详细的使用文档。

这里还有一个youtube视频系列，可以帮大家系统学习：
[Awesome CSS with Bourbon, Neat, Bitters & Refills!](https://www.youtube.com/playlist?list=PLfdtiltiRHWErI0VSxDCbeDyEJm_kVt3p) 国内的朋友记得翻墙哦。

BTW：随着项目的进行，我会逐步写出对于一些架构和工具选择总结的文章。

