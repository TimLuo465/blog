---
title: javascript之数组的合并
tags:
  - Javascript
  - 技术文章
date: 2016-05-16 22:03:32
---

#### **开胃菜**

  在js中，合并数组有一个现成的方法<span style="font-size: 14px;">**concat**, <span style="font-size: 16px;">它的作用便是连接多个数组，并产生一个新的数组。</span></span><!--more-->

  当然，你也可以不传数组，传数组元素，一样也是可以的。

{% codeblock lang:javascript %}
var a = [], b = [1,2,3,4,5];
console.log(a.concat(1,2,3,4,5)); // [1,2,3,4,5]
console.log(a.concat(b, b)); // [1,2,3,4,5,1,2,3,4,5]
{% endcodeblock %}

#### **主菜**

  上面说得的是正常情况下合并数组采取的方法，那当合并的数组过大，而<span style="font-size: 14px;">**concat**<span style="font-size: 16px;">方法已经不太适用。因为生成新的arrayObject也是要占用内存滴๛ก(ｰ̀ωｰ́ก)。</span></span>

  那么用什么方法能够解决这么个问题呢？

  没错，就是<span style="font-size: 14px;">**push**<span style="font-size: 16px;">，</span><span style="font-size: 16px;">使用<span style="font-size: 14px;">**push**</span></span><span style="font-size: 16px;">可以向数组末尾添加多个元素。利用这一点我们可以完成两个数组的合并。</span></span>

{% codeblock lang:javascript %}
var a = [1,2,3,4], b = [5,6,7,8];
a.push.apply(a, b);
console.log(a); // [1,2,3,4,5,6,7,8]
{% endcodeblock %}

#### **结语**

  在javascript中，类似这种的小技巧不胜枚举。让人人大开眼界的同时，也让我们对js更加的着迷。