---
title: 生成hexo目录的工具 koa-generator
date: 2018-09-26 13:51:02
tags:
---

###Features

Express-style
Support koa 1.x（supported）
Support koa 2.x（koa middleware supported,need Node.js 7.6+ ,babel optional）

Installation
$ npm install -g koa-generator
with 2 commands

koa (Support koa 1.x)
koa2 (Support koa 2.x)
Quick Start 1.x
The quickest way to get started with koa is to utilize the executable koa(1) to generate an application as shown below:

Create the app:

$ koa /tmp/foo && cd /tmp/foo
Install dependencies:

$ npm install
Rock and Roll

$ npm start
Quick Start 2.x
The quickest way to get started with koa is to utilize the executable koa2(1) to generate an application as shown below:

Create the app:

$ koa2 /tmp/foo && cd /tmp/foo
Install dependencies:

$ npm install
Rock and Roll

$ npm start
more detail see koa2-demo

Command Line Options
This generator can also be further configured with the following command line flags.

-h, --help          output usage information
-V, --version       output the version number
-e, --ejs           add ejs engine support (defaults to jade)
    --hbs           add handlebars engine support
-n, --nunjucks      add nunjucks engine support
-H, --hogan         add hogan.js engine support
-c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
    --git           add .gitignore
-f, --force         force on non-empty directory
目前选项-c还没有实现

Git Branch Details
master = koa generator
tpl = koa 1.x template
tpl_2.x = koa 2.x template
License