---
title: javascript解释器内部实现系列(1) - parseInt
date: 2017-05-15 13:24:12
tags:
  - javascript
category:
  - javascript解释器内部实现系列
---
#### 概述
parseInt是一个出现频率很高的方法，主要用途就是将字符串转化为整数。更官方点：

> 解析一个字符串参数，并返回一个指定基数的整数。
<!--more-->
#### 用法

> parseInt(string, radix);

第二个参数是基数，也就是进制。如二进制，十进制等等，其取值范围是2到36。

parseInt返回整数否则NaN。

#### 庖丁解牛parseInt(str, radix)

来看看javascript的解释器做了哪些事情

##### 第一步：将str转化为字符串， ToString(str)

{% codeblock lang:javascript %}
//等同于js
String(str)
{% endcodeblock %}

##### 第二步：移除str起始位置的空白符，生成新的字符串S，即S=trimStart(str)

{% codeblock lang:javascript %}
// js中trimStart可实现为
str.replace(/^\s*/g, "")
{% endcodeblock %}

##### 第三步：确定字符串str的符号位，大致过程如下所示

{% codeblock lang:javascript %}
let sign = 1;
if (isNotEmpty(S)) { // 如果S不为空
    // 如果S的首字符为'-'
    if(S.indexOf('-') == 0) { 
        sign = -1;
    }
    // 如果S的首字符为'+'或者'-'，则移除首字符
    if(S.indexOf('-') == 0 || S.indexOf('+') == 0) {
        S = S.substring(1, S.length);
    }
}
{% endcodeblock %}

##### 第四步：确定基数radix的值

{% codeblock lang:javascript %}
let R = ToInt32(radix); //等同于js中的Number(radix)
let stripPrefix = true; 

if(R != 0) {
    if(R < 2 || R > 36) return NaN;
    if(R != 16) stripPrefix = false;
} else {
    R = 10; 
}

if(stripPrefix) { // 如果有前缀 即十六进制
    if( S.length >= 2 && (S.indexOf("0x") != -1 || S.indexOf("0X") != -1) ) {
        // 移除前两位字符
        S = S.substring(2, S.length);
        R = 16;
    }
    // 虽然EcMAScript 5中 只判断了0x前缀 但是有些浏览器对八进制'0'也有判断，并一直兼容着这一点
    // 所以在使用时，务必要明确给出radix的值
}
{% endcodeblock %}

##### 第五步：找到字符串S中第一个非R进制的字符，截取出该字符前的子串Z

{% codeblock lang:javascript %}
parseInt("123test", 10) // Z = "123"
parseInt("01112", 2) // Z = "0111"
parseInt("0x11g", 16) // Z = "0x11"

if(isEmpty(Z)) return NaN;
{% endcodeblock %}

##### 第六步：将Z表示为对应R进制的数学整数mathInt，计算最终结果

{% codeblock lang:javascript %}
let mathInt = ToMathInt(Z);
let number = Number(mathInt);
return sign x number;
{% endcodeblock %}

#### 需要注意的点

1. 可以使用isNaN来判断parseInt的结果是否为NaN
2. intValue.toString(radix)可以将整数转化为对应进制下的字符串

{% codeblock lang:javascript %}
(19).toString(16) // "13"
(19).toString(2) // "10011"
{% endcodeblock %}