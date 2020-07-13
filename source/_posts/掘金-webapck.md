---
title: 掘金-webapck
date: 2018-07-13 18:52:42
tags:
---
# 掘金webpack小册

## webpack 的概念和基础使用
我们在小册介绍中提到，webpack 是一个 JS 代码模块化的打包工具，藉由它强大的扩展能力，随着社区的发展，逐渐成为一个功能完善的构建工具。相信开始学习这个小册的同学们多多少少都能够理解为什么前端开发中会使用到 webpack，我们不再详细介绍 webpack 的使用背景，直奔本小节的主题。...

因为是作为项目依赖进行安装，所以不会有全局的命令，npm/yarn 会帮助我们在当前项目依赖中寻找对应的命令执行，如果是全局安装的 webpack，直接执行 webpack --mode production 就可以。

webpack 4.x 的版本可以零配置就开始进行构建，但是笔者觉得这个功能还不全面，缺少很多实际项目需要的功能，所以基本你还是需要一个配置文件，后边会详细讲解。

我们先来了解 webpack 中的一些基本概念。...


----------
### loader
webpack 中提供一种处理多种文件格式的机制，便是使用 loader。我们可以把 loader 理解为是一个转换器，负责把某种文件格式的内容转换成 webpack 可以支持打包的模块。

举个例子，在没有添加额外插件的情况下，webpack 会默认把所有依赖打包成 js 文件，如果入口文件依赖一个 .hbs 的模板文件以及一个 .css 的样式文件，那么我们需要 handlebars-loader 来处理 .hbs 文件，需要 css-loader 来处理 .css 文件（这里其实还需要 style-loader，后续详解），最终把不同格式的文件都解析成 js 代码，以便打包后在浏览器中运行。

当我们需要使用不同的 loader 来解析处理不同类型的文件时，我们可以在 module.rules 字段下来配置相关的规则，例如使用 Babel 来处理 .js 文件：...

```
module: {
  // ...
  rules: [
    {
      test: /\.jsx?/, // 匹配文件路径的正则表达式，通常我们都是匹配文件类型后缀
      include: [
        path.resolve(__dirname, 'src') // 指定哪些路径下的文件需要经过 loader 处理
      ],
      use: 'babel-loader', // 指定使用的 loader
    },
  ],
}...

```


----------
### plugin

在 webpack 的构建流程中，plugin 用于处理更多其他的一些构建任务。可以这么理解，模块代码转换的工作由 loader 来处理，除此之外的其他任何工作都可以交由 plugin 来完成。通过添加我们需要的 plugin，可以满足更多构建中特殊的需求。例如，要使用压缩 JS 代码的 uglifyjs-webpack-plugin 插件，只需在配置中通过 plugins 字段添加新的 plugin 即可：...

```
const UglifyPlugin = require('uglifyjs-webpack-plugin')

module.exports = {
  plugins: [
    new UglifyPlugin()
  ],
}
```


----------


## 搭建基本的前端开发环境

我们日常使用的前端开发环境应该是怎样的？我们可以尝试着把基本前端开发环境的需求列一下：

构建我们发布需要的 HTML、CSS、JS 文件
使用 CSS 预处理器来编写样式
处理和压缩图片
使用 Babel 来支持 ES 新特性
本地提供静态服务以方便开发调试...

### **html-webpack-plugin** 什么时候使用？

如果我们的文件名或者路径会变化，例如使用 [hash] 来进行命名，那么最好是将 HTML 引用路径和我们的构建结果关联起来，这个时候我们可以使用


----------

## 使用Plugin
	
webpack 中的 plugin 大多都提供额外的能力，它们在 webpack 中的配置都只是把插件实例添加到 plugins 字段的数组中。不过由于需要提供不同的功能，不同的插件本身的配置比较多样化。

社区中有很多 webpack 插件可供使用，而优秀的插件基本上都提供了详细的使用说明文档。更多的插件可以在这里查找：plugins in awesome-webpack。

下面通过介绍几个常用的插件来了解插件的使用方法。

### DefinePlugin

属于内置插件,可以使用webpack.DefinePlugin直接获取

**作用**

这个插件用于创建一些在编译时可以配置的全局常量，这些常量的值我们可以在 webpack 的配置中去指定

```
module.exports = {
  // ...
  plugins: [
    new webpack.DefinePlugin({
      PRODUCTION: JSON.stringify(true), // const PRODUCTION = true
      VERSION: JSON.stringify('5fa3b9'), // const VERSION = '5fa3b9'
      BROWSER_SUPPORTS_HTML5: true, // const BROWSER_SUPPORTS_HTML5 = 'true'
      TWO: '1+1', // const TWO = 1 + 1,
      CONSTANTS: {
        APP_VERSION: JSON.stringify('1.1.2') // const CONSTANTS = { APP_VERSION: '1.1.2' }
      }
    }),
  ],
}
```


----------

## 更好地使用 webpack-dev-server

在构建带吧并部署到生产环境之前，我们需要一个本地环境，用于运行我们开发的代码。这个环境相当于提供了一个简单的服务器，用于访问webpack构建好的静态文件，我们日常开发可以使用它来调试前端代码。


### webpack-dev-server 的基础使用
	
webpack-dev-server 是一个 npm package，安装后在已经有 webpack 配置文件的项目目录下直接启动就可以

```
npm install webpack-dev-server -g
webpack-dev-server --mode development 
```

