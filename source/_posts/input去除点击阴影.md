---
title: input去除点击阴影
tags:
  - css3
  - 技术文章
date: 2016-06-04 17:50:34
---

在移动端，点击input文本框时，会出现一坨阴影，好丑的说。

好吧，看看如何来消除吧。<!--more-->

#### 方法一

`
input {
  -webkit-tap-highlight-color:rgba(255,255,255,0);
}`

#### 方法二

`
input {
  -webkit-appearance: none;
}
`

#### 方法三

`
input {
  border: 0;
}`

<span style="font-size: 14px;">**<span style="color: #808000;">来瞅瞅这些属性具体是啥意思</span>**</span>。[戳这里](http://www.css88.com/webkit/-webkit-line-clamp/ "-webkit-tap-highlight-color")

&nbsp;