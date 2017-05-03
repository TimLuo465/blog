---
title: 自定义scrollbar的样式
tags:
  - css3
  - javascript
date: 2016-05-08 15:33:04
---

### 引言

webkit中支持拥有overflow属性的区域，列表框，下拉菜单，textarea的滚动条自定义样式。通过设置一些简单的样式，就能让你的滚动条变得高大上。<!--more-->

### 单刀直入

{% codeblock lang:css %}
::-webkit-scrollbar { /* 1 滚动条整体部分*/ } 
::-webkit-scrollbar-button { /* 2 滚动条两端的按钮*/ } 
::-webkit-scrollbar-track { /* 3 滚动条的内层轨道*/ } 
::-webkit-scrollbar-track-piece { /* 4 滚动条除内层方块的内层轨道*/ } 
::-webkit-scrollbar-thumb { /* 5 滚动条内滑动的方块*/ } 
::-webkit-scrollbar-corner { /* 6 x,y轴滚动条交汇处的边角*/ } 
::-webkit-resizer { /* 7 x,y轴滚动条交汇处调正大小的控件，类似于textarea调整大小的倒三角*/ } 
{% endcodeblock %}

![](/images/scrollbarparts.png "scrollbarparts")

一个简单的demo可以让你更容易上手，明白这些伪类都有着什么样的作用。

{% codeblock lang:css %}
::-webkit-scrollbar { width: 12px; }
::-webkit-scrollbar-track { 
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3); 
  border-radius: 10px; 
}
::-webkit-scrollbar-thumb { 
  border-radius: 10px; 
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.5); 
}
{% endcodeblock %}

当然，webkit同样有着一些伪类可以应用在这些样式上面。

> **:horizontal **适用于水平位置的滚动条。
> 
> **:vertical **适用于垂直位置的滚动条。
> 
> **:decrement **适用于按钮和轨道碎片。表示递减的按钮或轨道碎片，例如可以使区域向上或者向右移动的区域和按钮。
> 
> **:increment **适用于按钮和轨道碎片。表示递增的按钮或轨道碎片，例如可以使区域向下或者向左移动的区域和按钮。
> 
> **:start **适用于按钮和轨道碎片。表示对象（按钮 轨道碎片）是否放在滑块的前面。
> 
> **:end **适用于按钮和轨道碎片。表示对象（按钮 轨道碎片）是否放在滑块的后面。
> 
> **:double-button **适用于按钮和轨道碎片。判断轨道结束的位置是否是一对按钮。也就是轨道碎片紧挨着一对在一起的按钮。
> 
> **:single-button **适用于按钮和轨道。判断轨道结束的位置是否是一对按钮。也就是轨道紧挨着一对在一起的按钮。
> 
> **:no-button** 适用于轨道和是否运行轨道片的滚动条，即边缘，轨道结束的位置没有按钮。
> 
> **:corner-present **适用于所有滚动条，表示是否存在滚动条的角落。
> 
> **:window-inactive **适用于按钮和轨道。判断轨道结束的位置是否是一个按钮。也就是轨道紧挨着一个单独的按钮。