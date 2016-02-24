---
layout: post
title: Rails4新项目的选择2：前端框架，Simple_form, 后台任务，搜索和用户认证
---

![](/images/Bing_701.JPG)

Rails的世界各种开源的优秀解决方案非常多，项目开始的时候比较痛苦地就是要根据自己的原则和项目的特点来做出正确的选择。

##前端开发框架的选择
原本打算在新的项目中开始使用ES6，然后前端的开发框架选择容易上手的，轻量级的[vue.js](http://vuejs.org)。由于Vue.js只支持IE9以及以上的浏览器，考虑到新项目是面向国内用户的产品，用户中还是有很大一部分的IE8，以及以下的浏览器用户。所以，最后还是决定使用Jquery+[knockout.js](http://knockoutjs.com)这样的组合。

通过在论坛里面帖子[前端框架选择的一个现实的问题](https://ruby-china.org/topics/27898)评论了解到。jquery也有支持双向绑定的lib： [jquerymy.js](http://jquerymy.com)
还有就是在这种情况下，如果要做SPA的话，可以使用[backbone.js](http://backbonejs.org)，兼容性也是很好的。

##放弃Simple-form
不再使用simple-form,主要是感觉它提供的一些功能对现在的项目帮助不大，还有就是simple form的wrapper定制比较麻烦，也会把页面代码弄得很臃肿。在这种前提下还是使用rails自带的form_for就可以了。原则就是如果要使用的gem帮助不是很大，还是最优先用Ruby和Rails自带的一些方案。

##表单验证
尝试使用[client side validations](https://github.com/DavyJonesLocker/client_side_validations)这个gem，这样前后端尽可能共享验证的规则，减少自己写相关js代码。少些代码，少制造bug。

##Rails后台任务框架的选择：
网上已经有很多资料比较Sidekiq，Resque和DelayedJob。我自己也都有简单用过。
所以决定在项目开始时（流量不大）使用DelayedJob，简单易用，不要单独安装和配置Redis，同时使用ActiveJob提供的API,这样以后迁移到Sidekiq或者Resque也就不会很麻烦。

##搜索：Elasticsearch
本周也开始看一下搜索功能的实现，以前我做过几年的基于Lucene的搜索开发，搜索服务就准备用elasticsearch，发现相关的Rails的整合方案也很多。有官方的elasticsearch-rails,还有一个比较流行的是[searchkick](https://github.com/ankane/searchkick),看了一下感觉功能很强大，所以下周准备拿searchkick试试。

##用户认证：Devise
这个选择主要是因为Devise功能强大，相关文档也很多，之前在项目中也用过。
