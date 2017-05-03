---
title: '(0,eval)(''this'')的一些总结'
tags:
  - javascript
date: 2016-05-03 23:30:58
---

### 一、Global Object

> Global Object代表一个全局对象，js中不允许存在独立的函数，变量和常量，它们都是Global Object 的属性和方法，包括内置的属性和方法。但是Global Object实际并不存在，它是由window充当这个角色，并且这个过程是在js首次加载时进行的。

> 在一个页面中，首次载入js代码时会创建一个全局执行环境，这个全局执行环境的作用域链实际上只有一个对象，即全局对象（window），并用this来引用全局对象。 <!--more-->

### 二、(0, eval)('this')

(0, eval)('this')可以用于获取当前的global object。那么为什么是(0, eval)('this')呢?

1.  先从(0, eval)表达式开始

表达式中使用了_逗号操作符，_而逗号操作符会返回表达式的最后一位，即返回的是eval值，不是引用eval。而0则可以是任意字符串或数字，不影响结果。

2.  再说('this')

为什么要用'this'，而不是this，因为_严格模式_下，在匿名函数中的this是undefined。

{% codeblock lang:javascript %}
(function(){
'use strict'; 
  console.log(this); // undefined
})() 
{% endcodeblock %}

### 三、eval的直接调用和间接调用

直接调用的概念:

> 对eval函数的直接调用，就是表示为一个调用表达式，它满足以下两个条件：
> 
> 调用表达式中的成员表达式的结果应该是eval引用，而不是值。
> 
> 以该引用为参数调用抽象操作GetValue的结果，是15.1.2.1中定义的标准的原生函数。

    简单来说，eval('1+1')是一个直接调用，而(1,eval)('1+1')是一个间接调用。eval('1+1')就是一个调用表达式，包含了一个成员表达式(eval)和一个参数('1+1')。

    相反的，不是直接调用就是间接调用。

### 四、window.eval和window.execScript

    IE浏览器中有一个window.execScript功能。它的定义是"在所提供的语言中执行指定的脚本"，同时在全局范围内允许执行Javascript代码。

    window对象存在属性形式调用eval，这其实也是eval的间接调用，和(0,eval)('this')没什么两样。