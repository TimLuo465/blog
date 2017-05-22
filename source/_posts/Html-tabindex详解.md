---
title: Html tabindex详解
tags:
  - html
date: 2016-03-16 00:22:42
---

#### 用法和定义

tabindex 属性规定元素的 tab 键控制次序（当 tab 键用于导航时）。<!--more-->

#### 适用元素

&lt;a&gt;, &lt;area&gt;, &lt;button&gt;, &lt;input&gt;, &lt;object&gt;, &lt;select&gt; 以及 &lt;textarea&gt;。

#### 语法

`<element tabindex="number">`

**number**: 规定元素的 tab 键控制次序（1 是第一个，<span style="color: #333300;">**取值范围**</span>是[1~32767]）。

#### Tip

1.  将元素的tabindex设置为<span style="color: #ff6600;">'-1'<span style="color: #000000;">，按下Tab键时该元素就不能获得焦点。</span></span>
2.  当元素使用tabindex后，点击该元素，元素周围会出现边框(又一个bug 8-O )。对此我表示别用tabindex了。