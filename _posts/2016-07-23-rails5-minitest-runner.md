---
layout: post
title: Rails5为Minitest提供了更好用的Runner
---

![](/images/Bing_702.JPG)

之前有写过一篇在Rails4项目中使用minitest进行测试的文章: [minitest + capybara测试基于devise的用户注册](http://www.bigbing.net/2015/11/23/rails4-minitest-capybara-devise/)

### Rails5中新的runner

如果你已经开始使用Rails5，你可能已经发现当你运行`bin/rails -h`的时候，Rails提示你现在还支持使用`bin/rails test`来运行所有的测试。

这个新的runner提供很多非常好用的功能，而且有spring预加载的帮助，运行速度很快。

1. 可以通过指定行号来运行某一个测试，这对于跟sublime等开发工具的集成有很大帮助.(之前只能通过指定测试名称来运行单个测试)

		$ bin/rails test test/models/user_test.rb:27

2. 可以同时运行多个不在同一个文件中的测试

		$ bin/rails test test/models/user_test.rb:27 test/models/post_test.rb:42

3. 快速失败功能，通过`-f`参数来指定，当有一个测试失败时就马上停止测试。
4. 延迟显示错误信息，通过`-d`参数来指定，在运行完所有的测试后一起显示错误信息。
5. 指定`-b`显示详细的错误的调用信息
6. 运行匹配的测试，通过`-n`参数来制定匹配的字符串或者正则表达式

		$ bin/rails t -n "/create/"

7. 仍然可以通过`-s`来指定seed
8. 指定`-v`参数可以查看详细的测试运行过程，其中包括每个测试的运行时间，这样有助于找出运行慢的测试

### Sublime的集成

有了这样强大的runner，在Sublime的RubyTest插件中就可以设置相关的命令来运行当前文件的所有测试或者某个测试，这是我所喜欢的开发测试方式。

	"run_ruby_unit_command": "rails test {relative_path}",
    "run_single_ruby_unit_command": "rails test {relative_path}:{line_number}",


