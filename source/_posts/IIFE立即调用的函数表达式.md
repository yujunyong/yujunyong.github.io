---
title: IIFE 立即调用的函数表达式
date: 2016-04-10 23:01:18
tags: [js, iife]
---

# IIFE (Immediately Invoked Function Expression 立即调用的函数表达式)
> [函数声明和函数表达式](/2016/04/10/函数声明和函数表达式/)
* 函数声明是语句所以在后面加`()`会抛出`SyntaxError`的错误
* 函数表达式是表达式可以在它后面加`()`执行这个函数
* js解释器在遇到关键字`function`时，默认认为`function(){}`, `function foo(){}`这两种形式是函数声明，而不是函数表达式
* `()`, `&&`, `,`, `!`, `~`, `+`, `-`, new 可以明确指出上述的两种形式是函数表达式而不是函数声明

## 自执行函数表达式写法
``` javascript
// 下面2个括弧()都会立即执行

(function () { /* code */ } ()); // 推荐使用这个
(function () { /* code */ })(); // 但是这个也是可以用的

// 由于括弧()和JS的&&，异或，逗号等操作符是在函数表达式和函数声明上消除歧义的
// 所以一旦解析器知道其中一个已经是表达式了，其它的也都默认为表达式了
// 不过，请注意下一章节的内容解释

var i = function () { return 10; } ();
true && function () { /* code */ } ();
0, function () { /* code */ } ();

// 如果你不在意返回值，或者不怕难以阅读
// 你甚至可以在function前面加一元操作符号

!function () { /* code */ } ();
~function () { /* code */ } ();
-function () { /* code */ } ();
+function () { /* code */ } ();

// 还有一个情况，使用new关键字,也可以用，但我不确定它的效率
// http://twitter.com/kuvos/status/18209252090847232

new function () { /* code */ }
new function () { /* code */ } () // 如果需要传递参数，只需要加上括弧()
```

## 使用IIFE的好处
### 提升性能
减少作用域查找。使用IIFE的一个微小的性能优势是通过匿名函数的参数传递常用全局对象window、document、jQuery，在作用域内引用这些全局对象。JavaScript解释器首先在作用域内查找属性，然后一直沿着链向上查找，直到全局范围。将全局对象放在IIFE作用域内提升js解释器的查找速度和性能。

传递全局对象到IIFE的一段代码示例：
``` javascript
// Anonymous function that has three arguments
function(window, document, $) {

  // You can now reference the window, document, and jQuery objects in a local scope

}(window, document, window.jQuery); // The global window, document, and jQuery objects are passed into the anonymous function
```
### 有利于压缩
另一个微小的优势是有利于代码压缩。既然通过参数传递了这些全局对象，压缩的时候可以将这些全局对象匿名为一个字符的变量名（只要这个字符没有被其他变量使用过）。
``` javascript
// Anonymous function that has three arguments
function(w, d, $) {

  // You can now reference the window, document, and jQuery objects in a local scope

}(window, document, window.jQuery); // The global window, document, and jQuery objects are passed into the anonymous function
```

### 避免全局命名冲突
当使用jQuery的时候，全局的window.jQuery对象 作为一个参数传递给$，在匿名函数内部你再也不需要担心$和其他库或者模板声明冲突

## IIFE最佳实践
如果你有大量的JavaScript代码都在一段IIFE里，要是想查找IIFE传递的实际参数值，必须要滚动到代码最后。幸运的是，你可以使用一个更可读的模式：
``` javascript
(function (library) {

 // Calls the second IIFE and locally passes in the global jQuery, window, and document objects
 library(window, document, window.jQuery);

}

// Locally scoped parameters 
(function (window, document, $) {

// Library code goes here

}));
```
这种IIFE模式清晰的展示了传递了哪些全局对象到你的IIFE中，不需要滚动到长文档的最后
