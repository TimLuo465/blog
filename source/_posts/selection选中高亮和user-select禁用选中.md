---
title: '::selection选中高亮和user-select禁用选中'
date: 2017-04-27 20:35:23
tags:
  - css3
---

### ::selection

#### 介绍和使用

用于文档中被用户高亮的部分。

::selection中可以用的的css属性并不多,大略如下 <!-- more -->

{% codeblock lang:css %}
::selection { 
  color: #fff;
  background: #333;
  cursor: defualt;
  outline: none;
  text-decoration: underline;
  text-emphasis-color: #555;
  text-shadow: 1px 1px 2px #ddd; 
}
 {% endcodeblock %} 

> 其中text-emphasis-color, text-shadow存在兼容性的问题, ie下不兼容。

#### 兼容性

Chrome | Firefox(Gecko) | IE | Opera | Safari
--- | --- | --- | --- | ---
 √  |  √ (-moz)  | 9  | 9.5  | 1.1 
 
### user-select

#### 介绍和使用

控制用户选取的部分能否被选取

> value - none | text | all | element
> 由于user-select还属于非标特性，所以使用时需要加上前缀

{% codeblock lang:css %}
.user-select {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
}
 {% endcodeblock %}

#### 兼容性

目前主流浏览器都是支持user-select特性的，但ie需要10+