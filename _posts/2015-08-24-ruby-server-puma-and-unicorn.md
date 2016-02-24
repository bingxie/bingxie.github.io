---
layout: post
title: 快速选择Ruby Server：Puma和Unicorn
---

![](/images/Bing_698.JPG)

> 一句话：如果是Rails4之后的新项目，我会选择Puma，如果是之前的老项目继续使用unicorn。

### 使用Puma最主要有两个优点：

* Puma支持多线程，可以提升并发。
* Puma 省内存。 Unicorn 一个进程 120M - 140M，Puma 一个进程 80M-120M。

[Heroku已经推荐使用Puma，主要由于Unicorn容易受到慢客户端的攻击](https://devcenter.heroku.com/changelog-items/594)

如果决定使用Puma，下面的两片文章会比较有帮助。

[How Do I Know Whether My Rails App Is Thread-safe or Not?](https://bearmetal.eu/theden/how-do-i-know-whether-my-rails-app-is-thread-safe-or-not/)

[How To Set Up Zero Downtime Rails Deploys Using Puma and Foreman](https://www.digitalocean.com/community/tutorials/how-to-set-up-zero-downtime-rails-deploys-using-puma-and-foreman)


### 更多有用资料：

[Scaling Ruby Apps to 1000 Requests per Minute - A Beginner's Guide](http://www.nateberkopec.com/2015/07/29/scaling-ruby-apps-to-1000-rpm.html)

[Ruby Web服务器：这十五年](http://insights.thoughtworkers.org/ruby-web-server/)

[Github: awesome-webservers](https://github.com/planetruby/awesome-webservers)
