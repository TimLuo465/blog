---
title: 按Enter键后Form表单自动提交的问题
tags:
  - Html
  - 技术文章
date: 2016-03-15 23:50:01
---

无意中发现在form表单中input文本框中按下了enter键，不料页面竟然刷新了一下。(尼玛，bug呀!)

好吧，只能动手找找原因了。以下省略一万个字。。。。<!--more-->

> **主要原因是**当表单中只有一个input输入元素时，点击enter键，form表单会自动提交。

显而易见，从不同角度下手，solution还是有很多的。

1.  多加一个input元素设置display:none。
2.  利用事件阻止表单提交。
3.  等等。。。。

我的思路呢，是利用事件来做的，没什么别的理由，因为适合。

`
<form onsubmit="return false">
     <input type="text" name="test"/>
</form>
`