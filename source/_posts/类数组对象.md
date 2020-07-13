---
title: 类数组对象
---
# 类数组对象

**故事要从一行代码说起**
```
arr[0]
// 'tom'
```

请问这里的 arr 一定是一个数组吗？非也，它也可能是一个对象

```
let arr = {
  0: 'tom'
}
arr[0]
// 'tom'
```

前面我们讲过 **arguments** 这个对象

```
let say = function (){
	console.log(arguments)
}
say(1,2)
```

其大致结构如下

```
0: 1
1: 2
length: 2
```

我们称 arguments 为类数组对象，正是因为它有这个 length 属性，虽然不能用数组的方法，但是可以通过索引来访问，而且还可以将其转换成数组。

如果一个普通的对象加上一个 length 属性就可以变成一个类数组对象。

之所以今天要讲这个类数组对象，是因为 JavaScirpt 中有很多方法和处理跟类数组对象有关系，所以我们有必要搞清楚这个概念。

### 转换

ES6 的 Array.from() 可以将一个类数组对象转成数组

```
let say = function () {
  console.log(Array.from(arguments))
}
say(1, 2)
// [1, 2]
```

当然也可以用 ES5 的 slice

```
Array.prototype.slice.call(arguments)
```

接下来我们自己起来创建一个类数组对象

```
let mans = {
  0: 'tom',
  1: 'lucy',
  length: 2
}
Array.from(mans)
// ["tom", "lucy"]
```

如果 length 值和实际元素不相等呢？


```
let mans = {
  0: 'tom',
  length: 2
}
Array.from(mans)
// ["tom", undefined]
```

可以看到，如果 length 值大于实际元素的数量，不足的将用 undefined 填充，那么反过来呢？

```
let mans = {
  0: 'tom',
  1: 'lucy',
  length: 1
}
Array.from(mans)
// ["tom"]
```
可见，length 值是决定最终生成数组的长度的，多余的去掉，不足的用 undefined 补齐。好奇心驱使我们继续探索，0 和 1 这两个索引真的有用吗？

```
mans = {
  3: 'tom',
  1: 'lucy',
  length: 2
}
Array.from(mans)
// [undefined, "lucy"]

```

所以，这个值是真的有用的，决定了最终的填充索引。细心的同学可能会记得，还有另外一个转换 arguments 的方法

```
[...arguments]
```
那么这个展开运算符可以用到普通的类数组对象上吗？还是亲自测试一下

```
let mans = {
  0: 'tom',
  1: 'lucy',
  length: 2
};
[...mans]
// Uncaught TypeError: mans is not iterable
```

可以看到报错了，mans 不是可迭代的，也就证实了 arguments 除了是类数组对象，还是一个可迭代对象，而我们自定义的对象并不具备可迭代功能，所以不能使用展开运算符。

### 遍历

因为有 length 属性，所以我们可以用 for 循环进行遍历

```
let mans = {
  0: 'tom',
  1: 'lucy',
  length: 2
};
for (let i = 0; i < mans.length; i++) {
  console.log(i)
}
// 0
// 1
```

除了传统的 for 循环，其它遍历方法可以使用吗？
```
mans.forEach(item => {
  console.log(item)
})
// mans.forEach is not a function
```

因为 forEach 是数组的一个方法，只能用于数组，自然就不能用在类数组对象身上了。

```
for (let man of mans) {
  console.log(man)
}
// mans is not iterable
```

因为 for...of 也是需要应用到可迭代对象上的。

```
for (let index in mans) {
  console.log(index)
}
// 0
// 1
```

所以只有 for 和 for...in 可以用来遍历类数组对象。

检测

除了 arguments，DOM 元素节点集合也是一个类数组对象，打开任意一个网页，F12 输入下面的代码


```
document.getElementsByTagName('div')
```

可以看到里面有 length 属性，的确是类数组的结构，Array 也有 length 属性，难道这就不会是一个真的数组吗？

倒也是，这个时候就需要利用我们前面讲过的检测数组的方法了

```
Object.prototype.toString.call(document.getElementsByTagName('div'))
// [object HTMLCollection]
```

可以看到这确实不是一个数组。