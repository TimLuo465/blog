---
title: 前端性能优化之不执行css和js的情况下预加载文件
tags:
  - performance
  - 技术文章
date: 2016-06-27 23:07:56
---

#### 引言

前端优化中，性价比最好的应该算是静态资源的合并压缩了。既能减少http请求数量，又能减少响应大小。如果能做到预缓存这些静态资源，那简直不要太爽了。<!--more-->

但有时候在缓存静态资源的时候，会遇到css和js解析和执行的尴尬。那如果做到不执行的同时让css和js缓存呢。看看下面的做法，也许会对你有所帮助。

#### 先来看看css要做什么处理

我们需要用到link标签里面的一属性: media,这个属性规定了该css文件显示在什么设备上。那么我们可以这样写：
{% codeblock lang:html %}
<link rel="stylesheet" type="text/css" href="app.css" media="none"/>
{% endcodeblock %}

#### 再来看看JS要怎么做

js需要采用动态添加object元素的方式，具体如下：
{% codeblock lang:javascript %}
var o = document.createElement("object");
o.data = "jquery.js"
document.body.appendChild(o);
{% endcodeblock %}

需要注意的是，在没有设置样式的情况下，加载的object元素会出现在页面上，需要视情况设置其width,height,display等样式。但<span style="color: #ff0000;">不能设置display:none</span>，否则将无法缓存js文件。

#### 为什么这样做

这样做的目的是为了满足场景下预缓存一些过大的js的情况。假设加载的js问价存在依赖，正常的方式会在加载的同时执行js，导致报错。这样就无法做到缓存的目的。

在用create object element的方式后，假设你的server已经做过cache-control,max-age,ETag这些设置。那么当下次再次请求这个文件时，就不再是200，而是from cache.