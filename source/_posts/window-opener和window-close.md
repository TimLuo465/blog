---
title: window.opener和window.close
tags:
  - 技术文章
date: 2016-07-25 22:30:40
---

#### 引言

  今天逛了下虎牙直播，无意间点了第三方登录，登录成功后第三方的新tab页自动关闭，直播页面刷新。顿时，觉得挺有意思的，然后自己做了个简单实现。<!--more-->

#### 热身

*   **window.close**

 方法 close() 将关闭有 window 指定的顶层浏览器窗口。某个窗口可以通过调用 self.close() 或只调用 close() 来关闭其自身。

 只有通过 JavaScript 代码打开的窗口才能够由 JavaScript 代码关闭。这阻止了恶意的脚本终止用户的浏览器。
*   **window.opener**

 opener 属性是一个可读可写的属性，可返回对创建该窗口的 Window 对象的引用。

 opener 属性非常有用，创建的窗口可以引用创建它的窗口所定义的属性和函数。


#### 牛刀小试

假设在A页面点击链接打开了B页面，B页面可以这样做

{% codeblock lang:javascript %}
window.opener.location.reload();
window.close();
{% endcodeblock %}

需要注意的是跨域的问题，也就是说如果A是一个http而B是一个https网站，直接这样子是不行的。