---
layout: post
title: Rails4新项目的测试：minitest + capybara测试基于devise的用户注册
categories: [Tech]
tags: [rails4, minitest, capybara]
---

![](/images/Bing_702.JPG)

> 没有测试就会让写程序的开发者没有自信

这次的新项目没有想当然的选择之前一直在用Rspec测试框架，主要是我感觉Rspec里面有太多的magic，在helper文件中很多tricks, 很快相关的配置文件就非常大。这篇Blog说的跟我的体会很像 [7 REASONS I'M STICKING WITH MINITEST AND FIXTURES IN RAILS](http://brandonhilkert.com/blog/7-reasons-why-im-sticking-with-minitest-and-fixtures-in-rails/)

minitest是Rails4后默认支持的测试框架，另外rails也提供了很多方便和强大的assert方法。可以参考[官方文档](http://guides.rubyonrails.org/testing.html)

下面我就总结和分享一下，Rails4新项目中minitest和capybara的集成和配置：

## Gemfile

{% highlight ruby %}
group :test do
  gem "minitest-reporters"

  gem 'capybara'
  gem "connection_pool"
  gem "launchy"
  gem "selenium-webdriver"

  gem "mocha"
  gem "poltergeist"
  gem 'phantomjs', :require => 'phantomjs/poltergeist'
end
{% endhighlight %}

* `minitest-reporters` 是为了美化minitest的测试结果输出地，有颜色，有百分比。
* `mocha` 是一个方便进行mock和stub的工具
* 其他几个都是跟capybara相关的工具，具体会在配置中看到。

## test_helper中添加的配置内容

在这里创建一个继承自新ActionDispatch::IntegrationTest的FeatureTest类，然后引入Capybara::DSL，作为Feature测试的父类。

{% highlight ruby %}
require "mocha/mini_test"

require "capybara/rails"
require "capybara/poltergeist"

# 注释掉默认的driver，打开后方便切换driver测试，也可以在代码中指定current_driver
# Capybara.default_driver = :selenium
# Capybara.default_driver = :poltergeist

Capybara.javascript_driver = :poltergeist

class FeatureTest < ActionDispatch::IntegrationTest
  # 可以在功能测试中使用capybara的DSL
  include Capybara::DSL

  # 默认的失败测试提示不是很有用，扩展后方便调试
  def assert_content(content)
    assert page.has_content?(content), %Q{Expected to found "#{content}" in: "#{page.text}"}
  end

  def refute_content(content)
    refute page.has_content?(content), %Q{Expected not to found "#{content}" in: "#{page.text}"}
  end
end

# See: https://gist.github.com/mperham/3049152
class ActiveRecord::Base
  mattr_accessor :shared_connection
  @@shared_connection = nil

  def self.connection
    @@shared_connection || ConnectionPool::Wrapper.new(:size => 1) { retrieve_connection }
  end
end
ActiveRecord::Base.shared_connection = ActiveRecord::Base.connection
{% endhighlight %}

## 测试确认邮件

如果要测试邮件的内容，内容中一般都会有链接，所以需要在config/environments/test.rb中加上：
{% highlight ruby %}
config.action_mailer.default_url_options = { host: '127.0.0.1', port: 3000 }
{% endhighlight %}

如果使用了delayed_job可以在其配置中加上：
{% highlight ruby %}
Delayed::Worker.delay_jobs = !Rails.env.test?
{% endhighlight %}
表明测试环境下，邮件任务都是直接发送出去的

## 用户注册和确认邮件测试的样例代码

在test目录下面，新建features目录，专门存放网站功能测试代码
`test/features/user_authentication_test.rb`

{% highlight ruby %}
require 'test_helper'

class UserAuthenticationTest < FeatureTest
  setup do
    ActionMailer::Base.deliveries.clear
  end

  test 'user can sign up with email and password' do
    visit new_user_registration_path

    assert_content "注册帐号"

    within '#new_user' do
      fill_in 'user[email]', with: 'test_user@gmail.com'
      fill_in 'user[password]', with: 'abcd1234'

      click_button '创建新帐号'
    end

    assert_equal sign_up_success_path, page.current_path

    assert_content "test_user@gmail.com"

    test_confirm_email
  end

  private

  def test_confirm_email
    assert_not ActionMailer::Base.deliveries.empty?

    cf_email = ActionMailer::Base.deliveries.last

    assert_equal ["support@guangchuan.com"], cf_email.from
    assert_equal ['test_user@gmail.com'], cf_email.to
    assert_equal '确认信息', cf_email.subject
    assert_match /Confirm my account/, cf_email.body.to_s
  end
end
{% endhighlight %}

所以可以看到用minitest写的代码都非常直接，都是纯ruby代码。等代码增多后，可以按照ruby的方式来组织，重用和重构。
