---
layout: post
title: 理解Rails5中Controller和Integration测试
---

![](/images/Bing_706.JPG)

这篇文章首先介绍Rails5中controller测试的变化，然后通过类图来分析Rails5中的`IntegrationTest`相关的类组织结构。通过理解相关的类和模块的关系来帮助我们写出更好的测试。

在之前的文章[minitest + capybara测试基于devise的用户注册](http://www.bigbing.net/2015/11/23/rails4-minitest-capybara-devise/)中，也是通过创建继承自`ActionDispatch::IntegrationTest`的`FeatureTest`类，然后引入Capybara的DSL模块，来方便我们创建其他Feature Test或者叫User Acceptance Test。

## 第一部分：Rails5中controller测试的变化

### 1. ActionController::TestCase废弃掉了

在Rails5中Controller测试都是继承自`ActionDispatch::IntegrationTest`类，而不是之前的`ActionController::TestCase`。如果还想继续使用，那么可以使用这个gem: [rails-controller-testing](https://github.com/rails/rails-controller-testing)。

### 2. assigns和assert_template也废弃掉了

在Rails4的controller测试中`assigns`方法用于获得action中的实例变量然后进行验证，`assert_template`用于验证action最后渲染了指定的template。

在Rails5中，controller测试强调的更多的是action的处理结果，比如响应的状态和响应的结果。

如果你还是想使用上面这两个方法，还是可以在[rails-controller-testing](https://github.com/rails/rails-controller-testing)这个gem中找到他们。

### 3. 移走assert_select等方法
	
`assert_select`等验证响应的HTML内容的方法，已经移到单独的[rails-dom-testing](https://github.com/rails/rails-dom-testing)这个gem中。

所以，如果是我要验证页面的内容和样式等，还是通过capybara来进行精确的操作和验证。

### 4. 使用URL而不是Action来发送请求

在Rails4中，通过action的名字来发送请求（说实话我一直很不习惯）

	class PicturesControllerTest < ActionController::TestCase
	
	  def test_index_response
	    get :index
	    assert_response :success
	  end
	end
	
而在Rails5中，要换成URL（多直观），否则就会抛出异常：`URI::InvalidURIError: bad URI`

	class PicturesControllerTest < ActionDispatch::IntegrationTest
	
	  def test_index
	    get pictures_url
	    assert_response :success
	  end
	end
	
### 5. HTTP的请求方法中必须使用关键字参数

在Rails5中，HTTP请求的方法参数必须明确指定关键字，比如params，flash等。这样会让代码更加清楚。请看例子：

	class PicturesControllerTest < ActionDispatch::IntegrationTest
	
	  def test_create
	    post picture_url, params: { picture: { name: "sea" } }
	    assert_response :success
	  end
	end

## 第二部分：理解ActionDispatch::IntegrationTest类和相关module

在继承了`ActionDispatch::IntegrationTest`类的Controller测试中，我们可以使用很多方便的helper方法和大量用于结果验证的assertions方法。
比如跟响应相关的：

	json = response.parsed_body # 解析json格式的响应结果
	assert_response :success # 验证成功的请求
	
还有跟路由routing相关的

	assert_routing({ method: 'post', path: '/pictures' }, controller: 'pictures', action: 'create')
	assert_recognizes({ controller: 'pictures', action: 'index' }, '/')
	
如果你要测试文件上传功能，Rails提供了非常方便的方法`fixture_file_upload`。但是你会发现你无法在controller中直接使用，你需要引入`ActionDispatch::TestProcess`模块。有点奇怪？

所以，为了搞清楚这些helper方法和assertions的来源，也方便我们日后查询相关的文档，我会通过下面的类图来理解IntegrationTest这个类。

![](/images/integration_test.png)


### 1. ActiveSupport::TestCase

首先在Rails中我们有`ActiveSupport::TestCase`类，它继承自`Minitest::Test`类，然后像`ActionDispatch::IntegrationTest`, `ActionView::TestCase`, `ActiveJob::TestCase`等我们自己的测试需要继承的测试基类，都继承自`ActiveSupport::TestCase`类。同时你会发现，我们自己的model的测试都是直接继承自`ActiveSupport::TestCase`类。

所以在我们的测试中，可以直接使用minitest提供的一些assertions，比如常见的：

	assert_equal( expected, actual, [msg] )
	assert_includes( collection, obj, [msg] )
	assert_instance_of( class, obj, [msg] )
	
由于`ActiveSupport::TestCase`引入了`ActiveSupport::Testing::Assertions`模块，所以我们可以使用非常方便的方法
	
	assert_difference(expression, difference = 1, message = nil, &block)
	# 例如
	assert_difference 'Article.count' do
  	  post :create, params: { article: {...} }
	end
	
	assert_no_difference(expression, message = nil, &block)
	
另外还有两个比较有用的被引入的模块是`ActiveSupport::Testing::FileFixtures`和`ActiveSupport::Testing::TimeHelpers`。他们分别提供了访问fixtures下面的文件和修改测试时间的方法

	file_fixture(fixture_name)
	
	travel(duration, &block)
	travel_back()
	travel_to(date_or_time)

### 2. ActionDispatch::IntegrationTest
	
接下来我们再来看看`IntegrationTest`这个类，它首先通过引入`Integration::Runner`模块，从而一起引入了`ActionDispatch::Assertions`模块，然后Runner中运行测试的时候，会创建`Integration::Session`类的实例，`Integration::Session`引入了`Integration::RequestHelpers`模块,所以我们就可以使用像get, post, put等HTTP请求相关的方法。
具体的这些请求相关的方法，参考文档： [ActionDispatch::Integration::RequestHelpers](http://api.rubyonrails.org/classes/ActionDispatch/Integration/RequestHelpers.html)

了解了发送请求的方法后，我们再来看看`ActionDispatch::Assertions`模块，它只是引入了另外两个重要的module：`ActionDispatch::Assertions::ResponseAssertions`和 `ActionDispatch::Assertions::RoutingAssertions`

`ResponseAssertions`中提供了常用的a`ssert_redirected_to`和`assert_response`方法
	
	assert_redirected_to login_url
	assert_response :redirect
	
`RoutingAssertions`中提供了上面展示过的：`assert_generates`， `assert_recognizes`， `assert_routing`方法，用于进行路由Routing相关的测试。


所以，理解了`IntegrationTest`的结构后，我们就知道为什么我们还需要引入`ActionDispatch::TestProcess`来测试文件上传，详细的如何测试Carrierwave的文件上传功能我会在下一篇文章中介绍。


