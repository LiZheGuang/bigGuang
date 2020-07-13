---
title: mongoose文档翻译
date: 2018-09-25 18:28:45
tags: mongoose 文档翻译
---
#  mongoose 文档翻译

[TOC]

###  Getting Started（入门）

First be sure you have MongoDB and Node.js installed.

Next install Mongoose from the command line using npm:

```
 npm install mongoose
```

现在说我们喜欢的小猫，想要记录我们在MongoDB中见过的每只小猫。我们需要做的第一件事是在项目中包含mongoose，并在我们本地运行的MongoDB实例上打开与测试数据库的连接。

```
// getting-started.js
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
```
我们与在localhost上运行的测试数据库有挂起的连接。如果我们成功连接或发生连接错误，我们现在需要收到通知：
```
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  // we're connected!
});
```

一旦我们的连接打开，我们的回调就会被调用。为简洁起见，我们假设所有以下代码都在此回调中。

使用Mongoose，一切都来自Schema。让我们来参考它并定义我们的小猫。

```
var kittySchema = new mongoose.Schema({
  name: String
});
```
到现在为止还挺好。我们有一个带有一个属性name的模式，它将是一个String。下一步是将模式编译为模型。

```
var Kitten = mongoose.model('Kitten', kittySchema);
```
模型是用于构造文档的类。在这种情况下，每个文档都将是一个小猫，其属性和行为在我们的模式中声明。让我们创建一个小猫文件，代表我们刚刚在人行道上遇到的小家伙：

```
var silence = new Kitten({ name: 'Silence' });
console.log(silence.name); // 'Silence'
```

小猫可以喵喵叫，让我们来看看如何在我们的文档中添加“说话”功能：

```
// NOTE: methods must be added to the schema before compiling it with mongoose.model()
//注意：在使用mongoose.model（）编译方法之前，必须将方法添加到模式中
kittySchema.methods.speak = function () {
  var greeting = this.name
    ? "Meow name is " + this.name
    : "I don't have a name";
  console.log(greeting);
}

var Kitten = mongoose.model('Kitten', kittySchema);
```

添加到模式的methods属性的函数将编译到Model原型中并在每个文档实例上公开：
```
var fluffy = new Kitten({ name: 'fluffy' });
fluffy.speak(); // "Meow name is fluffy"
```
我们有说话的小猫！但是我们还没有向MongoDB保存任何内容。可以通过调用其save方法将每个文档保存到数据库中。如果发生任何回调，回调的第一个参数将是错误。

```
fluffy.save(function (err, fluffy) {
    if (err) return console.error(err);
    fluffy.speak();
  });
```
我们想要显示我们见过的所有小猫,我们可以通过我们的Kitten模型访问所有小猫文档。
```
Kitten.find(function (err, kittens) {
  if (err) return console.error(err);
  console.log(kittens);
})
```
我们刚刚将数据库中的所有小猫记录到控制台。如果我们想按名称过滤我们的小猫，Mongoose支持MongoDB丰富的查询语法。


```
Kitten.find({ name: /^fluff/ }, callback);
```

这将搜索具有以“Fluff”开头的name属性的所有文档，并将结果作为小猫数组返回给回调。祝贺这是我们快速启动的结束。我们使用Mongoose在MongoDB中创建了一个模式，添加了一个自定义文档方法，保存和查询的小猫。转到指南或API文档了解更多信息。

### Guides 指南

Mongoose指南提供有关Mongoose核心概念的详细教程，并将Mongoose与外部工具和框架集成在一起。

#### Schemas

如果您还没有这样做，请花一点时间阅读快速入门，了解Mongoose的工作原理。如果您要从4.x迁移到5.x，请花点时间阅读迁移指南。

**Defining your schema （定义架构）**  

Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the shape of the documents within that collection.

Mongoose中的所有内容都以Schema开头。每个模式都映射到MongoDB集合，并定义该集合中文档的形状。

```
var mongoose = require('mongoose');
  var Schema = mongoose.Schema;

  var blogSchema = new Schema({
    title:  String,
    author: String,
    body:   String,
    comments: [{ body: String, date: Date }],
    date: { type: Date, default: Date.now },
    hidden: Boolean,
    meta: {
      votes: Number,
      favs:  Number
    }
  });
```

允许的SchemaTypes是：

- String
-	Number
-	Date
-	Buffer
-	Boolean
-	Mixed
-	ObjectId
-	Array
-	Decimal128
-	Map

[**SchemaTypes 的更多信息**](https://mongoosejs.com/docs/schematypes.html#schematypes)

模式不仅定义了文档的结构和属性的转换，还定义了文档实例方法，静态模型方法，复合索引和称为中间件的文档生命周期钩子。

Creating a model 创建模型


