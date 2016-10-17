---
layout: post
title: 顺利通过Code Test的Checklist
---

![](/images/Bing_709.JPG)

最近在找新的工作，澳洲这里几乎所有招Ruby工程师的公司都会要求你先做一个Code Test。就是给你一个需求，用代码实现。
Code Test通过后，才会进入面对面的面试，这其中还会有`Pair Programming`，所以一定要自己好好实践。下面是我总结的一些经验：

### 需求

* 要很好的理解需求，不要忽略一些细节
* 不要忘记一些非功能性的需求（比如要提供github地址，或者发送zip包）

### 面向对象设计

* 要符合一些基本的OOD的原则， 比如`Single Responsibility Principle`, `Open Close Principle`
* 充分利用语言的特性，设计好类，接口以及抽象和继承的层次
* 使用常用的设计模式：`Factory Pattern`，`Template Pattern`，`Observer Pattern`，`Command Pattern`，`Null Ojbect`
* 设想一两个需求变更的场景，验证自己的代码能够很容易的扩展

### 系统和架构的设计

* 尽量采用MVC分层架构模式，有`Data Model`，`Business Model`，`Controller`， `Display/Runner Class`

### 异常的处理

* 自定义异常，然后做好异常的处理
* 提供友好的错误的提示

## Test：

* 尽可能采用TDD，有清楚的commits，体现代码的更新过程
* 每个测试尽量保证一个assertion
* 保证足够高的测试覆盖率，100%是可以有的，可以使用`simplecov`来帮助
* 要有集成测试，有正常的流程测试和非正常的流程测试
* 一定要测试好各种edge cases
* 提供易用的`rake` tasks，方便运行相关的测试，比如单元测试，集成测试等

### 良好的接口设计

* 非常容易安装和执行，减少需要的依赖（比如尽量少的gems，Ruby版本）
* 有很好的命令行接口和帮助说明
* 要有一份`README`，说明如何安装和使用

### 注意代码质量和规范：

* 按照产品级别的代码要求自己
* 采用`Rubocop`进行质量检查
* 不要遗留任何的`TODO`
* 方法尽可能短小，不要超过5行

最后祝所有找工作的朋友好运！

