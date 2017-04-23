---
title: Enter键跳到下一步的问题
tags:
  - 技术文章
date: 2016-03-27 23:59:40
---

> **引言**

大致的问题是这样的，focus在input元素中按enter键要跳到<span style="color: #808080;">**_下一个元素_**</span>上。而下一个元素有这么三种:<!--more-->

1.  input输入框，只需要focus。
2.  select元素，需要展开select元素中的项。
3.  a元素，button的样式，需要触发功能事件。
> **吐槽**

问题提出来了，是时候扔一波砖头了。从上面三个问题来看貌似也没有那么难嘛，你看第一个给下一个next元素加个focus事件不就行么。第二个，额。。容我想想，对了! 给select加个size属性。哈哈，我太XX机智了(我不会告诉你我是刚百度来的)。第三个trigger一个click事件不就可以么。看起来问题很快就解决了啊。

好了，看我立马把code给你码来。你瞅瞅，瞅瞅ios上，让你focus，键盘都不弹出来，你让我输什么啊！再瞅瞅select拉得老长，给谁看啊！android上我点next为啥不好使呢，你这写的什么鬼。

以上纯属吐槽，下面才是正文 :lol: 

> **我是正文，真的！**

#### 一、第一个问题

思路是对的，给next元素加focus。在按了go键(相当于PC上的enter)ios上它真能跳到下一个元素上，但是仅仅是跳，并没有focus，只是边上有选中的框。这时候需要用到touchstart事件。

`
$(element).on("touchstart", function(){   
  $(element).next().focus(); 
}); 
$(element).trigger("touchstart");
`

运行下看看效果，android上面是好的，再瞅瞅ipad上面好像也可以了，但。。为嘛手机上不行。好吧你没有看错就是不行。

#### 二、第二个问题

给select添加size是可以做到展开的效果，但是需要考虑长宽等等因素。其实让select展开来，还是有别的方法的。

`
var e = document.createEvent("MouseEvents");
e.initMouseEvent("mousedown", true, true, window); 
element.dispatchEvent(e);
`

上面方法只适合chrome浏览器和android, ipad。iphone手机上再次被<span style="color: #ff0000;">KO</span>!

#### 三、第三个问题

这个问题只是trigger一下click事件就好，问题不太大。也就没什么好说的。

> **总结**

文章结束还是没能解决在iphone手机上focus弹出键盘的问题，还是想说在webview上控制系统键盘弹出还是相当无奈的。