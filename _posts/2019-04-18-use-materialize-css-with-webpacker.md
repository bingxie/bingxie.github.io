---
layout: post
title: 使用Wepacker安装和使用Materialize CSS
categories: [Tech]
tags: [rails, webpacker, materialize css]
---

![](/images/materialize-css-starter.png)
_快速应用Starter Template的效果_

`Materialize CSS`是一个比较新的基于`Google Material Design`的前端框架，界面非常简洁，而且很容易使用。

其中，在最新发布的`1.0`版本中，去掉了对于`JQuery`的依赖，这是一个很大进步。因为在`Rails5`以后也是移除了`JQuery`，而且接下来在我的项目中，我会开始使用`React.js`作为前端的开发框架。

下面就来介绍一下通过`Webpacker`安装和使用`Materialize`的步骤：

准备工作：
在`app/javascript/`下面创建`stylesheets`目录以及`application.scss`文件：

```
$ mkdir app/javascript/stylesheets
$ touch app/javascript/stylesheets/application.scss
```

然后我们需要`import`刚才创建的`scss`文件，让`Webpacker`帮我们编译`scss`文件

```javascript
# app/javascript/packs/application.js

import '../stylesheets/application'
```

在页面中引入`Webpacker`帮我们编译的`application.js`和`application.css`文件

```erb
# app/views/layouts/application.html.erb

<%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
<%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
```

使用`yarn`安装`materialize-css`，

```
yarn add materialize-css
```

我们可以在`node_modules/materialize-css`下面找到安装好的文件
下面我们要把`materialize.js`加入到`application.js`中

```javascript
# app/javascript/packs/application.js

import 'materialize-css/dist/js/materialize'
```
然后到`application.scss`中引入`materialize.css`

```scss
# app/javascript/stylesheets/application.scss

@import ‘materialize-css/dist/css/materialize’;
```

最后我们还需要在`application.html.erb`中加入`Materialize`依赖的字体和图标

```html
# app/views/layouts/application.html.erb

<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```

安装配置到这里就完成了，接下来就是找一些Materialize的实例，应用到网站上吧。
更多的细节可以参考我完成的commit： [Github commit](https://github.com/bingxie/investment-radar/commit/b58632eb40ec2fe9e42976f5cb48f8a8e0f5987f)
