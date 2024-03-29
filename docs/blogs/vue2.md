---
title: vue2
date: 2023-03-23
categories:
  - 前端
tags:
  - vue
---

**Vue 的响应式原理**
数据驱动视图的过程：1、数据变化 2、生成 vdom 节点，使用 diff 算法比较 vdom 节点的变化 3、更新视图
Vue 的响应式原理是核心是通过 Object.defindeProperty 完成的，他为所有的 data 的属性绑定 get 和 set 方法，当读取 data 中的数据时自动调用 get 方法，当修改 data 中的数据时，自动调用 set 方法，
检测到数据的变化，会通知观察者 Wacher，重新 render 当前组件（子组件不会重新渲染）,生成新的虚拟 DOM 树，Vue 框架会遍历并对比新虚拟 DOM 树和旧虚拟 DOM 树中每个节点的差别，并记录下来，最后，加载操作，将所有记录的不同点，局部修改到真实 DOM 树上

**vue 双向数据绑定原理**
Vue 的双向数据绑定是通过数据劫持和发布/订阅模式相结合来实现的，可以实现数据的同步更新，同时避免了手动更新视图的操作。
首先要对数据进行劫持监听，所以我们需要设置一个监听器 Observer，用来监听所有属性。
如果属性发上变化了，就需要告诉订阅者 Watcher 看是否需要更新。
因为订阅者 Watcher 是有很多个，所以我们需要有一个消息订阅器 Dep 来专门收集这些订阅者，然后在监听器 Observer 和订阅者 Watcher 之间进行统一管理的。
还需要有一个指令解析器 Compile，替换模板数据或者绑定相应的函数（对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者 Watcher），此时当订阅者 Watcher 接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。

1. 实现一个监听器 Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者
2. 实现一个订阅者 Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
3. 实现一个解析器 Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。
4. 实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

**vue 模板编译的原理**
Vue.js 是基于模板的渲染机制来实现页面的渲染的。Vue.js 通过将模板编译成渲染函数的方式来进行模板的渲染。模板编译的过程主要包括以下几个步骤：

1. 解析模板字符串：Vue.js 将模板字符串解析成 AST（Abstract Syntax Tree）。
2. 静态优化：在编译过程中，Vue.js 会静态地分析整个模板，检测不需要更改的部分，将其优化成静态内容，这些内容不需要在每次重新渲染时都重新生成。
3. 代码生成：将 AST 转换成渲染函数。这个过程包括将每个 AST 节点转换成代码字符串并拼接成渲染函数。
4. 渲染函数：将生成的渲染函数执行后，会得到一个 VNode（Virtual DOM 节点）。

这些步骤将模板编译成渲染函数，用于生成 Virtual DOM，并更新到页面中，从而实现页面的渲染。在每次有数据变化时，Vue.js 会重新调用渲染函数，生成新的 VNode，通过对比新旧 VNode 差异，从而仅仅更新需要更新的部分，提高页面的渲染效率。

**描述组件渲染的过程**
Vue 组件渲染的过程是将组件的模板渲染成真实的 DOM 节点并插入页面中的过程。它大致分为以下几个步骤：

1. 创建 Vue 实例：在使用 Vue 框架时，需要创建 Vue 实例。Vue 实例是 Vue 应用的入口，它将负责管理各个组件之间的数据传递和状态管理。
2. 模板解析：在 Vue 组件中，使用 HTML 模板描述组件的结构和样式。当 Vue 实例被创建时，它会解析组件的模板，并将解析后的模板存储在内存中。Vue 使用 HTML 解析器将模板解析为 AST（抽象语法树）。
3. 模板编译：在 Vue 实例渲染组件时，Vue 将 AST 编译为渲染函数，渲染函数是一个返回 HTML 字符串的 JavaScript 函数。
4. 组件渲染：当 Vue 实例需要渲染组件时，它会使用渲染函数将组件渲染成 HTML 字符串。
5. 真实 DOM 生成：将 HTML 字符串转换成真实的 DOM 节点。Vue 使用虚拟 DOM 算法，将 HTML 字符串转化为真实的 DOM 节点，Vue 会尽量复用之前生成的 DOM 节点，以提高性能。
6. 真实 DOM 插入：将真实的 DOM 节点插入到页面中。Vue 将生成的真实 DOM 节点插入到指定的挂载点，完成组件渲染的过程。
