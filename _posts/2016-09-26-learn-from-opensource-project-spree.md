---
layout: post
title: 通过Spree开源代码学Ruby和Rails知识
categories: [Tech]
tags: [ruby, spree]
---

![](/images/Bing_708.JPG)

最近在做`Spree`的定制开发，翻看其中的代码，发现其中有下面这么一段代码信息量是非常的大。所以就想通过这一小段代码来好好学习一下相关的Ruby知识。

{% highlight ruby %}
# spree/api/lib/spree/api/engine.rb

def self.activate
  Dir.glob(File.join(File.dirname(__FILE__), "../../../app/**/*_decorator*.rb")) do |c|
    Rails.configuration.cache_classes ? require(c) : load(c)
  end
end

config.to_prepare &method(:activate).to_proc
{% endhighlight %}

就是这样的一段代码却包含了大量的Ruby和Rails的知识。其中最后一行<code>config.to_prepare &method(:activate).to_proc </code>是本文重点解释的对象，接下来我们就一个一个来学习。

### self.active

`self.activate`方法其实不难理解，这是一个类方法，会在引入Spree的应用中查找`app`目录下面文件名中带有`_decorator`的文件，这些文件都是用来定制扩展Spree功能的文件。在这里，找到后会判断是否设置了`Rails.configuration.cache_classes`这个选项，一般在开发环境中，该选项是false，这样当你修改代码后Rails会自动reload相关的文件。而在产品环境中，该选项为true，因为我们不会在产品环境修改代码让Rails自动reload。

后面`require`和`load`的最重要的区别就是，`require`如果发现该文件已经加载过后就不会重新载入，而`load`不管之前是否已经载入都会加载该文件。

重点来了，让我们看看下面这行信息量巨大的代码吧。一眼就看明白的同学，请自行绕过！

{% highlight ruby %}
config.to_prepare &method(:activate).to_proc
{% endhighlight %}

### method(:activate)
`method`方法来自于`Object`，会在当前的实例(上面的例子就是self)上查找通过参数指定的方法。找到后返回一个`Method`的实例，这个Method的对象就是一个封装了方法所属的对象以及该对象的实例变量的闭包。

### method(:activate).to_proc
接下来就是调用`Method`对象的`to_proc`方法，产生一个对应的`Proc`对象。

### &method(:activate).to_proc
在`Proc`对象前面加上`&`符号，作为参数传递，相当于给方法传递了一个`block`，然后方法内部通过`yield`调用`block`，例如：

{% highlight ruby %}
def to_prepare
  yield if block_given?
  puts 'Done prepare!'
end
{% endhighlight %}

相反，如果在接收的`block`参数前面加上`&`符号，那就相当于给方法传递了一个`Proc`的对象，然后方法内部通过`proc.call`来调用

{% highlight ruby %}
def config(&block)
  block.call
  puts 'done config!'
end

config do
  puts 'I am block!'
end
{% endhighlight %}

### config.to_prepare
这是来自Rails配置里面功能, 该配置是全局性的，会在所有的initializers运行之后运行。很重要的一点是，该配置中的代码，在production和test环境中默认只会执行一次，而在开发环境dev中，会在每次修改文件，重新发出请求时，Rails完成reload，在实际进行请求处理之前执行这段代码。（好绕的流程）

所以，你会看到上面的代码就是来重新加载一些定制Spree功能的代码。

在Rails中你还可以手动调用`ActionDispatch::Reloader.to_prepare`来实现同样地功能。

另外还有一个与之对应的`ActionDispatch::Reloader.to_cleanup`,区别是该`callback`会在请求处理完成后执行。

上面提到开发环境的不同，主要是受到`config.cache_classes`配置的控制。开发环境dev下，该属性为`false`，所以Rails会发现有文件修改后，自动realod。

### 实例

介绍完这些基本的知识点后，你可能会觉得还是有点糊涂，为什么Spree中要这么做呢？

这种做法其实在Ruby的世界是一种常见的用法，主要就是利用闭包的特点,把相关的逻辑更好的组织和封装。关于这一点上面Spree的例子不是很完美。

下面有给出一段利用闭包中封装的实例变量的例子。

{% highlight ruby %}
class MethodTest
  class << self
    attr_accessor :root_path

    def activate
      puts @root_path
      puts helper
    end

    def helper
      "I am helper method. #{@root_path}"
    end
  end
end


def to_prepare
  yield if block_given?
  puts 'Done prepare!'
end

MethodTest.root_path = '/root'
to_prepare &MethodTest.method(:activate).to_proc
{% endhighlight %}
