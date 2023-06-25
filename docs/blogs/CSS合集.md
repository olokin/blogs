---
title: CSS合集
date: 2023-03-23
categories:
  - 前端
tags:
  - css
---

**盒模型**

盒模型是浏览器绘制一个块级盒子时采用的标准，分为 W3C 的标准盒模型和 IE 的怪异盒模型
区别在于宽度 widt 和高度 height 的定义范围
W3C 标准盒模型: width = content 的宽度，height = conetnt 的高度。
IE 怪异盒模型: width = conent 的宽度 + 左右 padding + 左右 border, height = content 的高度 + 上下 padding + 上下 border

**src 和 href 的区别**

1. src 会加载外部资源并引用到当前页面中，比如 script 的 src 会加载一个 js 脚本，img 的 src 会加载一张图片，而 href 表示当前页面和外部资源建立链接。
2. src 会替换当前内容，比如 script 的 src 会用加载到的内容替换 script 标签，href 不会。
3. src 会暂停其他资源的下载和处理，而 href 可以并行加载，比如 link 的 href 可以并行加载多个 css 文件。
4. href 是 Hypertext Reference 的缩写，表示超文本引用。用来建立当前元素和文档之间的链接。href 目的不是为了引用资源，而是为了建立联系，让当前标签能够链接到目标地址

**对于重绘和重排（回流）的理解，哪些操作会触发重绘和重排**

重绘: DOM 的修改导致了样式的变化时，却并未影响其几何属性时，会发生重绘，比如 background-color、color、outline、visibility 属性发生改变

重排: DOM 的修改引发了 DOM 几何尺寸的变化时，会发生重排，比如 width、height、margin、padding、display 等属性发生改变。

重绘和重排的关系：在发生重排的时候，浏览器会构建重排部分元素的渲染树，相当于会重新设置外观属性，会引发重绘，所以重排一定会引起重绘

如何减少重绘和重排：

1. 浏览器会维护一个重绘和重排的队列，等队列中的操作到了一定的数量或时间间隔，会进行一次批处理，让多次的重绘重排变成一次重绘重排。
2. 尽量用类名改变样式，而不要一个一个设置样式属性。
3. 对于那些复杂的动画，对其设置 position: fixed/absolute，尽可能地使元素脱离文档流，从而减少对其他元素的影响
4. 避免设置多项内联样式

**什么是 BFC，如何产生 BFC 以及 BFC 的作用**

Block format context - 块级格式化上下文，是文档中一块独立渲染的区域，内部元素的渲染不会影响边界以外的元素。

BFC 渲染规则:

1. 内部的块盒子会在垂直方向上一个接一个排列
2. 同一个 BFC 的两个相邻的盒子的 margin 会重叠
3. 每个元素的左外边距与包含块的左边界相接触（从左到右），即使浮动元素也是如此
4. BFC 的区域不会与 float 的元素区域重叠
5. 计算 BFC 的高度时，浮动子元素也参与计算

如何触发 BFC:

1. html 根元素
2. 浮动元素(float 值不为 none)
3. overflow 值不为 visible、clip 的块元素
4. position 是绝对定位(absolute)和固定定位(fixed)元素
5. display 值为 inline-block、table、table-cell、table-caption 的元素
6. display 值为 flex 或 inline-flex 或 grid 或 inline-grid 元素的直接子元素(如果它们本身既不是 flex、grid，也不是 table 容器)

BFC 的应用:

1. 防止相邻兄弟元素 margin 重叠
2. 清除内部浮动
3. 自适应多栏布局

**定位有哪几种，以及每个定位的特点**

静态定位(static)、相对定位(relative)、绝对定位(absolute)、固定定位(fixed)、粘性定位(sticky)
静态定位: 不脱离文档流、默认值
相对定位: 不脱离文档流，相对于自身位置定位
绝对定位: 相对于非 static 定位的最近一层的定位元素定位
固定定位: 脱离文档流，相对于视口的位置定位。
粘性定位: 相对定位和固定定位的结合，在到达指定位置之前为相对定位，到达指定位置后变为绝对定位。
