---
layout: post
title: 为 VS Code定制 Rspec 的 formatter
---

![](/images/Bing_713.JPG)

最近我已经完全从 `Sublime` 转到了 `VS Code`，`VS Code` 的各种功能做的还是非常不错的. 在这里也向大家推荐。

其中我最喜欢的就是集成了 terminal，然后在做 Rails 相关的项目开发时，通过 Rspec 的插件可以快速的在集成的终端中运行测试和调试代码(`pry debug`)。

这样在开发的过程中就不需要离开编辑器去调试代码，极大的缩短了BDD和 TDD 的循环时间。

由于我还是喜欢把 terminal 放到下面，然后也不能给 terminal 很大的空间，当测试失败的时候，会打印出很长的 backtrace，这时候经常就需要滚动鼠标才能查看失败的信息或者异常信息. 效果如下图所示:
![Rspec default formatter](/images/rspec-default-formatter.png)

这时想到了，Rspec 支持自定义的formatter，这样我就可以把输出的 backtrace 简化，保留我可能感兴趣的出错文件和测试信息.看上去的效果如下。
![Rspec custom formatter](/images/rspec-custom-formatter.png)

详细的`custom_formatter.rb`代码请见: [gist link](https://gist.github.com/bingxie/e25e1a43fe75aea38b3aa30b9b0536e4)

**使用方法:**
把 `custom_formatter.rb` 下载到本地，然后配置 Rspec 插件，设置如下:

```json
"ruby.specCommand": "spring rspec --require ~/custom_formatter.rb --format CustomFormatter"
```

### Tips

* 在 VS Code 的 terminal 中可以，按住 Command 键然后点击带有行号的出错文件信息，以快速的打开文件
* VS Code的 Markdown 插件也很棒，让我也不再需要单独的 markdown 编辑器