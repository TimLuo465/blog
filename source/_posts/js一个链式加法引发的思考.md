---
title: js一个链式加法引发的思考
tags:
  - 技术文章
date: 2016-03-31 23:48:13
---

> 前言

起因是碰到这样一道题目，完成一个连加的函数，使得其可以实现如下的要求:

`
add(1)(2) //3
add(1)(2)(3) //6
var addTwo = add(2);
addTwo //2
addTwo + 5 //7
addTwo(3) //5
addTwo(3)(5) //10`

<!--more-->

> 分析与思考

根据这道题目，其实可以很快的想到这是一道闭包的题目，并且add方法中必须返回一个函数。这样才能实现连续调用。初步分析可以一个雏形:

`
function add(n){
  var fn = function(m) {
   return add(n + m);
  }
  return fn;

}
`

但是题目中的结果是整数，而上面的返回的是function。要怎样才能让它可以完成累加并且返回一个整数呢？

经过一番折腾，终于找到了这么个神奇的办法，仰天笑三声。

办法很简单其实只需要重写fn的toString或valueOf方法就可以让fn顺利完成累加同时可以返回结果。

所以只需要在fn方法下添加:

`
fn.toString = function() {
  return n;
 }`

或者

`
fn.valueOf = function() {
  return n;
 }`

好吧，既然问题解决了。那么问题又来了，toString和valueOf都可以解决这个问题，那么他们的区别是什么呢？

> toString和valueOf的区别

#### 一、toString

当对象需要转换为字符串时，会调用它的toString()方法。而默认情况下，每个对象都会从Object上继承到toString()方法。

Function的原型方法中含有toString()，覆盖了Object中的toString()方法，它返回的是当前函数源代码的字符串。

#### 二、valueOf

<span style="color: #993300;">valueOf()</span>方法用来把对象转换成原始类型的值（数值、字符串和布尔值）。

默认情况下, valueOf() 会被每个对象Object继承。每一个内置对象都会覆盖这个方法为了返回一个合理的值，

如果对象没有原始值，valueOf() 就会返回对象自己。而此时javascript调用的是Objcet的toString()方法。

#### 三、示例

`
function A(){};
A.toString = function() {
  return 1;
}
alert(A); // 1
A.valueOf = function() {
  return 2
};
console.log(A + 2); // 4
"A".toString() // "A"
"A".valueOf() // "A" 这里javascript会在内部调用toString()
`

上面的示例只是一部分，更多的需要自己去体会，通过自己demo弄清楚toString和valueOf的区别。