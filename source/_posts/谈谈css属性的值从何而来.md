---
title: 谈谈css属性的值从何而来
date: 2017-05-06 11:23:40
tags:
    - css
---
#### 概述
---
从文档的解析到文档树的构建，再到元素属性的取值，这之间所发生都是我们值得去了解的。而接下来所要谈的便是元素属性取值这一步。

#### 属性计算的四步

---

元素的取值主要分为四个步骤:
1. **指定值Specified value**， 取自样式层叠 (选取样式表里权重最高的规则), 继承 (如果属性可以继承则取父元素的值)，或者默认值。
2. **计算值Computed value**， 如em,百分比转化为像素值，再如span 指定 position: absolute 后display 变为 block。
3. **应用值Used value**， 计算值的结果即为应用值，可以通过window.getComputedStyle来获取。
4. **实际值Actual values**，一般情况下应用值已经是用于渲染的值，但是如果所取的值无法应用于用户代理的环境。例如user Agent强制使用黑白色来代替彩色，那么近似值将取而代之。

计算值与应用值的区别
> CSS 2.0 只定义了 计算值 computed value 作为属性计算的最后一步。 CSS 2.1 引进了定义明显不同的的应用值，这样当父元素的计算值为百分数时子元素可以显式地继承其高宽。 对于不依赖于布局的 CSS 属性 (例如 display, font-size, line-height)计算值与应用值一样，否则就会不一样:

- background-position
- bottom, left, right, top
- height, width
- margin-bottom, margin-left, margin-right, margin-top,
- min-height, min-width
- padding-bottom, padding-left, padding-right, padding-top
- text-indent

> 注: 只有text-indent为默认继承，其他这需要使用inherit关键字。

#### 获取元素的属性值

---

在上面已经提到过window.getComputedStyle可以得到元素的属性值，那具体用法如何。

##### 用法

{% codeblock lang:javascript %}
var style = window.getComputedStyle(element, [pseudoElt]);
// 返回的样式是一个实时的 CSSStyleDeclaration 对象，当元素的样式更改时，它会自动更新本身。
{% endcodeblock %}

##### 例子

{% codeblock lang:html %}
<style>
    h3::after {
        content: "rocks!";
    }
</style>

<h3>generated content</h3> 

<script>
    var h3 = document.querySelector('h3'), 
    result = getComputedStyle(h3, '::after').content;
    console.log(result); // rocks
</script>
{% endcodeblock %}

##### 兼容性

Feature | Chrome | Firefox (Gecko) | IE | Opera | Safari 
---|---|---|---|---|---
基本| (Yes) | (Yes) |	9|	(Yes)|	(Yes)
伪为元素 | (Yes) |	(Yes)|	11|	未实现| (Yes)

#### inherit与initial关键字

---

inherit和initial都可以作为css属性的值，且都适用于所有的css属性。

##### 区别

inherit为继承，继承自父元素属性的计算值(computed value)

initial为初始值，即为user Agent中属性的默认值。

inherit基本兼容所有主流浏览器(IE8+)，而initial唯独ie不兼容。所以这一点需要注意。