---
layout: post
title: 本周总结13-18-07-2015
---

![](/images/Bing_696.jpg "")

###部署
本周发现产品环境发布后，自动重启unicorn失败，发现是在老的版本的目录下执行的`bundle exec unicorn`命令。

eye官网上找到的解决办法：
[https://github.com/kostya/eye/wiki/Run-ruby-apps-that-use-a-separate-bundle-than-eye-itself](https://github.com/kostya/eye/wiki/Run-ruby-apps-that-use-a-separate-bundle-than-eye-itself)
{% highlight ruby %}
if File.exist? File.join(working_dir, 'Gemfile')
  clear_bundler_env
  env 'BUNDLE_GEMFILE' => File.join(working_dir, 'Gemfile')
end
{% endhighlight %}

方法二：手动创建/bin/unicorn
{% highlight ruby %}
#!/usr/bin/env ruby
require_relative '../config/boot'
load Gem.bin_path('unicorn', 'unicorn')
{% endhighlight %}

这样就可以保证Unicorn会和应用使用同样地环境，包括gemfile的依赖


###网站安全：
本周发现网站访问有日志：`/POST_ip_port.php`

简单地方法可以用：`DDOS Deflate`

###搜索：Sphinx的模糊搜索问题

Sphinx通过`enable_star`和`min_infix_len`来配置实现英文的模糊搜索，但是这个特性会导致索引过大，同时也会降低搜索性能。
所以，考虑到我做的网站是中文网站，所以就去掉英文模糊搜索功能。详细请看：
[http://wyeworks.com/blog/2009/4/20/wildcard-search-with-thinking-sphinx](http://wyeworks.com/blog/2009/4/20/wildcard-search-with-thinking-sphinx)

###邮件服务：从Postmark切换到Mandrill
本周Postmark邮件服务出现问题，导致网站给用户发送的确认邮件都收不到，用户无法完成注册，这个对网站很致命。
所以我立即换到另外的服务Mandrill，主要原因是Postmark的服务跟不上，可能是时区的原因，我的反馈没有得到及时的解决。
Mandrill上手配置很容易，同时每个月如果少于2000封的话就免费，这个很适合新的网站。
另外补充一句，另外一个选择SendGrid的注册流程比较繁琐，还要验证网站。

###资源分享：
同时分享发现的一个简单易懂的CSS layout教程：
http://learnlayout.com/
