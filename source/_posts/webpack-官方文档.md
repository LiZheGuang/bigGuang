---
title: webpack-官方文档
date: 2018-07-13 18:54:44
tags:
---
#### webpack

###clean-webpack-plugin

你可能已经注意到，由于过去的指南和代码示例遗留下来，导致我们的 /dist 文件夹相当杂乱。webpack 会生成文件，然后将这些文件放置在 /dist 文件夹中，但是 webpack 无法追踪到哪些文件是实际在项目中用到的。

通常，在每次构建前清理 /dist 文件夹，是比较推荐的做法，因此只会生成用到的文件。让我们完成这个需求。

clean-webpack-plugin 是一个比较普及的管理插件，让我们安装和配置下。


----------
![Alt text](./1531383837459.png)


----------

###使用 source map

当 webpack 打包源代码时，可能会很难追踪到错误和警告在源代码中的原始位置。例如，如果将三个源文件（a.js, b.js 和 c.js）打包到一个 bundle（bundle.js）中，而其中一个源文件包含一个错误，那么堆栈跟踪就会简单地指向到 bundle.js。这并通常没有太多帮助，因为你可能需要准确地知道错误来自于哪个源文件。

为了更容易地追踪错误和警告，JavaScript 提供了 source map 功能，将编译后的代码映射回原始源代码。如果一个错误来自于 b.js，source map 就会明确的告诉你。

source map 有很多不同的选项可用，请务必仔细阅读它们，以便可以根据需要进行配置。

对于本指南，我们使用 inline-source-map 选项，这有助于解释说明我们的目的


----------
###模块热替换
模块热替换(Hot Module Replacement 或 HMR)是 webpack 提供的最有用的功能之一。它允许在运行时更新各种模块，而无需进行完全刷新。

**问题**

模块热替换可能比较难掌握。为了说明这一点，我们回到刚才的示例中。如果你继续点击示例页面上的按钮，你会发现控制台仍在打印这旧的 printMe 功能。

这是因为按钮的 onclick 事件仍然绑定在旧的 printMe 函数上。

为了让它与 HRM 正常工作，我们需要使用 module.hot.accept 更新绑定到新的 printMe 函数上：

```
  import _ from 'lodash';
  import printMe from './print.js';

  function component() {
    var element = document.createElement('div');
    var btn = document.createElement('button');

    element.innerHTML = _.join(['Hello', 'webpack'], ' ');

    btn.innerHTML = 'Click me and check the console!';
    btn.onclick = printMe;  // onclick 事件绑定原始的 printMe 函数上

    element.appendChild(btn);

    return element;
  }

 let element = component(); // 当 print.js 改变导致页面重新渲染时，重新获取渲染的元素
 document.body.appendChild(element);

  if (module.hot) {
    module.hot.accept('./print.js', function() {
      console.log('Accepting the updated printMe module!');
     document.body.removeChild(element);
     element = component(); // 重新渲染页面后，component 更新 click 事件处理
     document.body.appendChild(element);
    })
  }
```

##### HMR 修改样式表

借助于 style-loader 的帮助，CSS 的模块热替换实际上是相当简单的。当更新 CSS 依赖模块时，此 loader 在后台使用 module.hot.accept 来修补(patch)  标签。

所以，可以使用以下命令安装两个 loader ：

`npm install --save-dev style-loader css-loader`

![Alt text](./1531394549181.png)


----------

### tree shaking

ree shaking 是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块系统中的静态结构特性，例如 import 和 export。这个术语和概念实际上是兴起于 ES2015 模块打包工具 rollup。

```
webpack.config.js

mode:"development" //development不压缩  //production压缩
```


----------
### 生产环境构建

在本指南中，我们将深入一些最佳实践，并且使用工具，将网站或应用程序构建到生产环境中。


----------


### 代码分离
代码分离是 webpack 中最引人注目的特性之一。此特性能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。

#### 入口起点(entry points)
这是迄今为止最简单、最直观的分离代码的方式。不过，这种方式手动配置较多，并有一些陷阱，我们将会解决这些问题。先来看看如何从 main bundle 中分离另一个模块：


----------
# 概念

### Heading