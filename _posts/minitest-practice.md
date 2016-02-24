---
layout: post
title: Rails4新项目的测试：minitest实践
---

BACKTRACE=1 bin/rake test test/models/article_test.rb

If existing migrations required modifications, the test database needs to
be rebuilt. This can be done by executing bin/rake db:test:prepare.


sublime minitest + sping


mocha gem: for mocking or stubbing


group :test do
  gem "capybara"
  gem "connection_pool"
  gem "launchy"
  gem "minitest-reporters"
  gem "mocha"
  gem "poltergeist"
 # gem "shoulda"
 # gem "test_after_commit"
end

使用minitest就是尽可能采用ruby的方式，包括context可以使用类或者分多个文件
不要用shoulda matchers，之前项目很多matchers测试导致很慢


Fixtures:
All data is loaded only once, before the test suite runs.

config.transactional_fixtures = false

wait_for_ajax

setup teardown是每个test前后都会执行的。

# true false
assert
assert_not

assert_not_nil (test, message = nil) Ensures that obj.nil? is false.

Fixtures:

ERB:
  created_at: <%= Time.now - 1.weeks %>

<% 100.times do |n| %>
comment_<%= n %>:
  post: one
  email: <%= "user_#{n}@buildingrails.com" %>
  body: this is comment number <%= n %>
<% end %>

测试点：
If you get an error in your application, before fixing it in the code, first write a test (that will fail) and then when you fix it, the test will pass.

feature test

Test any callbacks such as before_create to ensure that your data is being generated as you expected.
