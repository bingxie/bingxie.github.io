---
layout: post
title: 2019 Rubyconf AU 精彩内容回顾
categories: [Tech]
tags: [rails, database]
---

![](/images/melbourne_forum.jpg)

## 大会总结

两天的2019年澳洲的 Ruby 技术大会顺利结束了，这次大会组织的还是很不错的，现场的秩序和投影和音效都不错，不足的地方就是由于场地选在比较老的Forum Melbourne，里面的座椅不是特别舒服。

本次大会的内容还是很丰富的，很多演讲者都是来自英国，美国。除了 Ruby 相关的技术主题，还有不少人文关怀的内容，比如 ADD（注意障碍），情绪管理和全球变暖的话题。

下面我来推荐几个我觉得非常不错的分享：

### The Case Of The Missing Method - A Ruby Mystery Story
Nadia Odunayo 通过一个非常好的故事解释了 Ruby 中的 singleton class 的秘密，以及讲解了 singleton class 在 DSL 的实际应用，Nadia 的演讲节奏控制的非常好，声音非常清楚。

[点击观看](https://youtu.be/aRkHqYDi_wQ)

### How to hijack, proxy and smuggle sockets with Rack/Ruby
Dávid Halász 通过一个非常具体和实际的例子，演示了 Ruby 中的 Socket 编程相关的技术。

[点击观看](https://youtu.be/dU7192V1mLI)

### Taming Monoliths Without Microservices
Kelly Sutton 在演讲中再次强调了如果要对 Monolithic Rails 应用进行拆分的话，重要的还是要把 Domain 划分清楚，以及如何恰当得处理 Domain 的界限和依赖，着重强调了不要有相互的依赖，依赖最好就是单向的。

[点击观看](https://youtu.be/-ovkkvvTiRM)

### Representations Count
Tom Stuart, 来自英国，Understanding Computation书的作者。他有着一口典型的英式口音。一听到这样的口音就让我想到了 IT Crowd. Tom 演示了通过不同的抽象表达，会对写代码解决问题产生巨大的影响。他用直观的图形解释了如何优雅的实现一个带负号的四则运算的实现。

[点击观看](https://youtu.be/ej-W956YQN0)

### A Branch in Time (a story about revision histories)
Tekin Süleyman 也是通过一个生动的故事讲解了写好 git commit 信息的重要性，视频中还会学到几个新的 git 命令可以帮助你快速查找历史，以及展示了一些写好 git commit 的最佳实践。

[点击观看](https://youtu.be/1NoNTqank_U)

### Views, from the top
Tim Riley 系统的介绍了 dry-rb view的主要功能和使用方法。演示了如何通过 dry-rb 来写一个更加 OO 的 view 层的代码。

[点击观看](https://youtu.be/VGWt1OLFzdU)

### Learn to make the point: data visualisation strategy
Mila Dymnikova 解释了数据可视化的重要作用，即使对于开发人员，在进行工作总结和汇报的时候也是非常重要的。她给出很具体的实现方法和一些好用的工具和服务。她的 PPT 做的非常生动。

[点击观看](https://youtu.be/4OkoJxR3jdM)

### Building APIs you want to hug with GraphQL
Tom Ridge 介绍了如何设计和实现一个好的 GraphQL APIs.

[点击观看](https://youtu.be/lyJebJuG_sk)

### What the hell is a JRuby?
Tom Gamon 非常简洁清楚得总结了几种不同的 Ruby 的实现，重点介绍了 JRuby 的实现以及在什么情况下我们可能会考虑在产品环境使用 JRuby。视频里面提到的两本书《Working with ruby threads》《Ruby Under a Microscope》确实都是非常不错的。

[点击观看](https://youtu.be/qJqC6xm-2PI)

### It's Down! Simulating Incidents in Production
Kelsey Pedersen通过一个具体的实例，讲解了如何通过 Feature Flag 模拟产品环境的故障，然后如何分析故障以及完善相关的工具和流程。非常具有实战意义。

[点击观看](https://youtu.be/NYZdambrG_I)

### Mechanically Confident
Adam Cuppy 演员出身的他有丰富的舞台经验，声音非常洪亮，非常能够调动现场的气氛，他总结了如何通过一些实践来建立好的 routine，从而不断取得进步然后建立自信。

[点击观看](https://youtu.be/Btsx9YzhJVM)

#### What were they thinking?"
Keith Pitty 凭借非常多年的经验，总结了软件开发方面的一些思考。

[点击观看](https://youtu.be/KiGbAbjYAjI)


更多视频请到Ruby Australia 的 Youtube 官网查看
