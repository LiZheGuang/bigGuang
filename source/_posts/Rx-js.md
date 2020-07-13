---
title: Rx.js
date: 2018-06-28 19:06:52
tags:
---
RxJS is ？
RxJS is a library for composing asynchronous and event-based programs by using observable sequences.

诞生的主要目的虽然是解决异步处理的问题，但并不表示Rx不适合同步的数据处理，实际上，使用RxJS之后大图片: https://images-cdn.shimo.im/1LJOPtcTrOUvS9RQ/image.png部分代码不需要关心自己是被同步执行还是异步执行，处理起来甚至更方便。

解决什么问题？
处理大量事件分发情况
提供丰富的异步事件处理操作
解决上面两个问题的情况下，保证代码拥有良好的可读性
RxJS采用的是函数响应式编程方式。
函数式编程
是一种编程思想， 把复杂问题分解成简单问题的组合。
根据Lambda演算，如果一个问题能够用一套函数组合的算法来描述，那就说明这个问题是可计算的，很自然，也就可以用编程语言的函数组合的方式来实现这样的计算过程。
声明式 - (命令式)
纯函数
函数的执行过程完全由参数决定，不会受除参数之外的任何数据影响
函数不会修改任何外部状态(实参，全局对象等)
数据不可变(	immutable 数据)
通过一点编程规范，再借助一些工具(Ramdajs, RxJS),就可以在JS中写出函数式的代码。
响应式编程

图片: https://github.com/LWHTarena/lwhtarena_note/raw/master/RxJS/013.jpg

其实大家已经在使用了，比如 EventTarget.addEventListener。

a = b + c
在传统方式下，这是一种一次性的赋值过程，调用一次就结束了，后面b和c再改变，a也不会变了。
而在Reactive的理念中，我们定义的不是一次性赋值过程，而是可重复的赋值过程，或者说是变量之间的关系：
a: = b + c
我们定义出这种关系之后，每次b或者c产生改变，这个表达式都会被重新计算。

b和c可以想象成数据流，以网页应用的前端领域为例，网页DOM的事件，可以看作为数据流；通过WebSocket获得的服务器端推送消息可以看作是数据流；同样，通过AJAX获得服务器端的数据资源也可以看作是数据流，虽然这个数据流中可能只有一个数据；网页的动画显示当然更可以看作是一个数据流。网页应用中众多问题其实就是数据流的问题。

Observable & observer
RxJS中的数据流就是Observable对象，Observable实现了下面两种设计模式：
观察者模式（Observer Pattern） 
迭代器模式（Iterator Pattern）

Hot/Cold Observable
订阅之前的数据还推送吗？Cold：Hot

操作符
返回一个全新的Observable对象。
对上游和下游的订阅及退订处理。
处理异常情况。
及时释放资源。
弹珠图
https://rxviz.com/
http://rxmarbles.com/

创建类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t17299/181/2464282714/291882/fbd3fcac/5af4077dN7ee89959.jpg


合并数据流类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t18256/184/2421553534/255994/f55e73a6/5af4077cN341ef44d.jpg

辅助类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t18220/131/2434894307/198139/43d64038/5af4077cNd1d675e9.jpg
过滤数据流类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t17305/163/2395368345/422739/f8c3a6b7/5af40778N186d8717.jpg

转化类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t17473/65/2401647117/246111/d1bde65b/5af4077dN11ce36fe.jpg

异常处理类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t17866/243/2414094033/89693/43035bff/5af40779Nf059566e.jpg


多播类
图片: https://img30.360buyimg.com/ebookadmin/jfs/t19456/245/2424296653/117947/48c2b9a/5af4077cNabe227bc.jpg
背压控制类（backpressure control）
错误处理类（error Handling） 
辅助工具类（utility） 
条件分支类（conditional&boolean） 
数学和合计类（mathmatical&aggregate）