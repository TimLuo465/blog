---
title: js之length故事知多少
tags:
  - Javascript
  - 技术文章
date: 2016-07-13 23:59:04
---

#### String.length

length属性会返回字符编码单位的数量。在js中字符串使用UTF-16编码，所以呢一个16比特的编码单位表示大部分常见的字符，也有使用两个单位表示不常见的字符。因此length返回值可能与字符实际数量不符。

而<span style="color: #808000;">**<span style="font-size: 14px;">String.length</span>**</span>返回1。<!--more-->

`
console.log("\\".length) //  1
console.log("".length) //  0
console.log("1234".length) //  4
console.log(String.length) //  1
`

#### Function.length

length属性返回function的形参个数。同上一样**<span style="font-size: 14px; color: #808000;">Function.length</span>**也是返回1。

length属性 <span style="color: #808000; font-size: 14px;">**Writable**</span>: false, **<span style="font-size: 14px; color: #808000;">Enumerable</span>**: false, **<span style="font-size: 14px; color: #808000;">Configurable</span>**: true.
`
console.log(Function.length) //  1
console.log( (function(a,b){}).length ) //  2
console.log( (function(){}).length ) //  0
`

#### arguments.length

length返回function实参的个数。

需要注意的是arguments是**<span style="color: #808000;">类数组</span>**对象而并非数组。
`
function isArguments() {
  return "callee" in arguments;
}

function toArray(arg) {
  return [].slice.call(arg);
}</pre><pre>function test(a, b) {
  console.log(arguments.length);
}
test(); // 0
test(1); // 1`

#### Array.length

length返回数组中元素个数。同样的<span style="color: #808000; font-size: 14px;">**Array.length**</span>为1。

length属性 <span style="color: #808000; font-size: 14px;">**Writable**</span>: true, **<span style="font-size: 14px; color: #808000;">Enumerable</span>**: false, **<span style="font-size: 14px; color: #808000;">Configurable</span>**: false.

writeable为true意味着你可以通过改变length长度来做到截断数组。
`
console.log(Array.length) // 1
console.log([1,2,3].length) // 3
var arr = [1,2,3];
arr.length = 2;
console.log(arr) // [1,2]`

#### length的取值范围

length的值是一个0-2<sup>32</sup>-1的整数。

`
console.log(new Array(Math.pow(2,32))) // Error...
`

那么如何判断一个数是一个合法的length呢
`
function isLength(n) {
  return n > 0 &&; n%1 == 0 && n <= Math.pow(2,32) - 1;
}`