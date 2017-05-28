---
title: jquery将滚动条滚动到指定的位置
tags:
  - javascript
date: 2016-03-18 00:29:09
---

这个问题解决起来并不困难，但前提是需要对scroll和元素的各种距离的属性有一定的了解。<!--more-->

> <span style="color: #333333;">**scrollTop()**</span>

#### 定义和用法

jquery中scrollTop()即可以用于设置匹配元素的滚动条的垂直偏移，也可以用于读取匹配元素集合的第一个元素的滚动条的垂直偏移。

*   <span style="color: #333300;">$(ele).scrollTop(value)</span>  value为设置的垂直偏移量。
*   <span style="color: #333300;">$(ele).scrollTop()</span> 可以得到垂直偏移量。

#### 深入了解

关于scrollTop的工作原理，主要有两点:

1.  当value没有值时，则读取元素的垂直偏移量，大致逻辑如下:

{% codeblock lang:javascript %}
// 判断value是否有值且是否为window或document对象 
if( value === undefined &amp;&amp; win) {    
  if("pageYOffset" in window){         
    return window["pageYOffset"]     
  } else {     
    return jQuery.support.boxModel ? 
      document.documentElement["scrollTop"] : win.document.body["scrollTop"]     
  } 
} else {    
  return ele["scrollTop"] 
} 
{% endcodeblock %}
以上是缩减的版本，只是简单阐述了jquery在没有value的情况下是怎么样取得元素的垂直偏移量。
2.  当value有值的情况下:

    1.  如果是window或document对象，则直接调用window.scrollTo(x, y)滚动到指定的位置。
    2.  如果不是则遍历匹配元素集合，并设置每个元素的滚动偏移值。
> <span style="color: #333333; font-weight: bold;">offset()</span>

#### 定义和用法

#### <span style="color: #808080;">    offset()</span>

      返回匹配元素集合中的第一个元素文档坐标。

<span style="color: #808080; font-weight: bold;">    offset( [options] )</span>

      设置每个匹配元素的文档坐标，参数options含有属性top或left对象，属性值是数值。例如$(ele).offset().top或者$(ele).offset({top: value})

#### 深入了解

    关于读取元素文档坐标，jquery会优先调用getBoundingClientRect()，说到getBoundingClientRect，有必要知道点。

<span style="color: #333300;">**<span style="color: #333333;">getBoundingClientRect</span> **</span>返回的是一个矩形对象，并包含有四个属性top,left,bottom,right。

{% codeblock lang:javascript %}
var rect = document.getElementById('element').getBoundingClientRect(); 
console.log(rect.top); // 元素上边距离页面上边的距离 
console.log(rect.bottom); // 元素下边距离页面上边的距离 
console.log(rect.left); // 元素左边距离页面左边的距离 
console.log(rect.right); // 元素右边距离页面左边的距离 
{% endcodeblock %}

对于浏览器兼容性来说，getBoundingClientRect还是很不错的。<span style="color: #473d3f;">不过需要注意的是</span><span style="color: #1f070c;">在IE中，在未添加到文档中的元素上调用方法getBoundingClientRect会抛出异常</span>。

好吧我承认啰嗦了半天都没有进入主题，这是毛病得改。。

> **将滚动条移动到指定位置**

其实经过以上的分析，已经不难得出解决方案了。

{% codeblock lang:javascript %}
var container = $(container),
    element = $(element);
$(container).scrollTop( $(element).offset().top - $(container).offset().top + $(container).scrollTop() );
{% endcodeblock %}

**<span style="color: #993300;">计算的规则是: 元素距离窗口上坐标 - 容器距离窗口上坐标 + 容器的垂直偏移量。</span>**