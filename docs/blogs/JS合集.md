---
title: JS合集
date: 2023-03-23
categories:
  - 前端
tags:
  - js
---

**script 标签中 defer 和 async 的区别**
async 和 defer 都是异步加载 js。
不同的是 async 是 js 只要一加载完毕就会马上执行 不管 html 有没有解析完毕，所以它有可能阻塞 html 解析。
defer 要等到 html 解析完毕之后才执行。所以不会阻塞 html 解析。

**箭头函数和普通函数的区别**

1. 箭头函数都是匿名函数
2. 剪头函数本身无 this，其 this 指向父级作用域的 this，并且 call,bind,apply 无法改变 this 的指向
3. 不可以被当做构造函数
4. 箭头函数没有参数，直接写一个空括号即可。参数只有一个，也可以省去包裹参数的括号。
5. 箭头函数不绑定 arguments，取而代之用 rest 参数...代替 arguments 对象
6. 箭头函数没有 prototype，普通函数有

**typeof 与 instanceof 的区别**

1. 返回结果类型不同：typeof 会返回一个变量的基本类型，instanceof 返回的是一个布尔值
2. 判断范围不同：instanceof 可以准确地判断复杂引用数据类型，但是不能正确判断基础数据类型；而 typeof 也存在弊端，它虽然可以判断基础数据类型（null 除外），但是引用数据类型中，除了 function 类型以外，其他的也无法判断
