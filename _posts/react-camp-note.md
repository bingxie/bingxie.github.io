React-Camp Note

各种.gitignore模板：
  https://github.com/github/gitignore  A collection of useful .gitignore templates

通过https://github.com/h5bp/html5-boilerplate/blob/master/src/index.html 项目来创建html模板文件

`BrowserSync` is a live editing tool. When you change a file, the browser automatically refreshes.

###npm
npm install
  --save adds browser-sync to package.json
  --save-dev 安装开发环境的依赖
  -d to make npm output more logging information.

npm ls
  see package dependencies

### Using GNU Make as a Front-end Development Build Tool
http://www.sitepoint.com/using-gnu-make-front-end-development-build-tool/

Use `PostCSS` to add features to standard CSS.
  @import to create a bundle from many CSS files.
  Use autoprefix to generate browser specific CSS property names for "experimental" features.

Use `autoprefixer` to add vendor prefixes automatically.
Include `normalizer.css` to fix browser inconsistencies.

Instead of using another link element to, let's use the postcss-import plugin to import normalize.css.

postcss --use autoprefixer --use postcss-import css/app.css --output bundle/app.css
==
cat css/app.css | postcss --use autoprefixer | postcss --use postcss-import | cat > bundle/app.css

Check Can I Use for browser compatibility

###Flex Layout
In ReactNative items are arranged from top-to-bottom by default, but in the browser the default is left-to-right.

如果不是使用Bootstrap这种大的框架，可以专门用http://www.responsivegridsystem.com/ 来做grid

#Day3

#Day4
position: static is the CSS default, but it's almost never useful. When using absolute positioning, you'd always have to remember to set the parent container to position: relative.

ReactNative changes the default to position: relative so we don't have to worry about this problem:

默认的position: static是没什么用的。
可以使用基于百分比的定位方式，这样可以用来实现响应式的设计，然后使用CSS的transform的translate方法进行准确地定位。

#Day5
GreenSock和Velocity.js是非常好用的，高性能的JS动画库
Google chrome devtools有很好用的观察页面动画，layout渲染的工具

Basically only CSS3 transform can be accelerated by the GPU. Any of the box model properties (top, left, width, height, padding, margin, border...) would trigger relayout and repaint.

#Day6
ScrollMagic


#day7
BEM CSS Naming Convention，是一种提高css可维护性的命名规范

flex-wrap: wrap; 实现自动换行

flex-grow: 1; flex-basis: 0 (or flex: 1 0;)通常作为一种范式来创建一个能够充满其父元素的容器，并且能够在有大量内容填充的时候，该容器不会超过其父元素的容器大小。
（不考虑自身的大小，填满父元素的空间）

flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

使用 perfect-scrollbar 插件是一种很好的替代浏览器内置的滚动条的方式。

flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
负值对该属性无效。

#day8
Set image's width to 100% to fill the container, while preserving aspect ratio.
Use percentage to divide a container into parts.
  flex-basis 20% 80%

#day9
使用Babel编译es6和JSX
  * Use let to replace var. Always.
  * Use => to replace anonymous functions. Always.
  * [Using ES6 With React](http://sike.io/courses/react/es6-with-react/)

* JSX is a syntactic sugar for `React.createElement`.
  [HTML2Jsx](https://facebook.github.io/react/html-jsx.html)

  Can put virtual DOM created JSX in variables, arrays, or return from function.
* Virtual DOM construction should be side-effects free.
  Given the same `this.state` and `this.props`, `render` should calculate the same virtual DOM.
* Parent component passes data to child components using the `this.props` property.
  Use `ES6` destructuring to make your code more concise.
* Use key to give child components a unique identity so React can reorder them.
  [Codepen Demo1](http://codepen.io/hayeah/full/dYvNXy)
  [Codepen Demo2](http://codepen.io/hayeah/full/KdWWPb/)

* Use `componentDidMount` to do setup after browser DOM is ready.
  有些第三方的js插件需要用到真实地Dom这个时候可以在组建加载完成后，通过ref来访问Dom
  Replace id with ref.

#day10
CommonJS
  require - A function used to load a module.
  module.exports - The module's exported content.

  // CommonJS
  let foo = require("foo");

  // ES6
  import foo from "foo";

  Babel will compile the ES6 import syntax to require.


Webpack is a tool that turns a CommonJS project into normal JavaScript that the browser can understand.

Usually require loads a file by its path. If it's a package name, NodeJS uses the require.resolve function to find which file to load.
通过npm安装的包，可以直接require包名，如果是自己项目的组件，那么就用相对路径引用

To be able to convert ES6/JSX to ES5, we'd also need to install the Webpack Babel plugin:
  npm install babel-loader --save-dev

###Source Map For Debugging

Webpack to generate source map, so Chrome can correlate between the JavaScript that runs in the browser, and the original source files you've written.

Add the -d option to the webpack command to enable the "development mode", which generates source map for your project.

minified js: webpack -p #-p option to enable production mode



