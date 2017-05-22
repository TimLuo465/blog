---
title: js event.relatedTarget
tags:
  - javascript
date: 2016-05-23 00:25:29
---

#### relatedTarget

碰巧有用到这个属性，这里就总结一下吧。<!--more-->

<span style="font-size: 14px; color: #808000;">**relatedTarget**</span>属性适用于**mouseover**，<span style="font-size: 14px;">**<span style="color: #808000;">mouseout</span> **</span>两个事件，并且在这两个事件中，表示着不同的意思。

<span style="color: #808000;">**mouseover**</span>中，relatedTarget表示鼠标指针移到目标节点上时所离开的那个节点。

<span style="color: #808000;">**mouseout**</span>中，relatedTarget表示离开目标时，鼠标指针进入的节点。

说再多，不如举个栗子。

先看看<span style="color: #808000; font-size: 14px;">**mouseover**</span>

<input id="mouseover" type="text" /><a>从我这移到文本框,试试看</a>

再看看<span style="color: #808000; font-size: 14px;">**mouseout**</span>

<input id="mouseout" type="text" /><a>从文本框移到我这,试试看</a>

需要说明的是，<span style="color: #808000; font-size: 14px;">**pointerover**</span>，<span style="color: #808000; font-size: 14px;">**pointerout**</span>事件也可以使用这个属性，但是由于这两个兼容性不是很好，所以就不举例子了。

<script>// <![CDATA[
document.getElementById('mouseover').onmouseover = function(event){
    alert(event.relatedTarget.tagName);
};
document.getElementById('mouseout').onmouseout = function(event) {
    alert(event.relatedTarget.tagName);
};
// ]]></script>