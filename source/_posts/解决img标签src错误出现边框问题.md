---
title: 解决img标签src错误出现边框问题
date: 2017-05-19 21:57:52
tags:
  - css
---
#### 概述
---
相信在这之前你一定试过**border: 0|none;** **outline: none;** 等等这些样式，但然并卵，归根结底还是浏览器的行为。那么到底该如何解决呢？
<!-- more -->
#### src为空或不存在
---

这种情况无疑是最好处理的，直接利用选择器对元素进行隐藏就可以了


{% codeblock lang:html %}
<img src="" width="100" height="100">
<img width="100" height="100">

img[src=""], /* src为空 */
img:not([src]) { /* 没有src属性 */
    visibility: hidden;
}
{% endcodeblock %}

#### 容器法
---
##### 思路

为**img**添加一个容器，利用父元素的overflow来将img的边框隐藏掉，但是缺点是需要额外增加一个DOM节点

{% codeblock lang:html %}
<div class="img-container">
  <img src="error-path" alt="no image" >
</div>
{% endcodeblock %}

##### 实现一

{% codeblock lang:css %}
.img-container{
  width:100px;
  height:100px;
  overflow:hidden;
}
.img-container img{
  width: 102px;
  height: 102px;
  margin:-1px;
}
{% endcodeblock %}
该方法中img的长宽需要比容器多2px，自己根据兼容性考虑使用**calc()函数** 回避hardcode

##### 实现二

{% codeblock lang:css %}
body {
  background-color: #fff;
}
.img-container {
  width: 100px;
  height: 100px;
  overflow: hidden;
  border: 1px solid #fff; /* 需要与背景色同色 */
  box-sizing: border-box;
}
.img-container img{
  width: inherit;
  height: inherit;
  margin: -1px;
}
{% endcodeblock %}
这个方法主要是利用父元素的border来遮掩img的border，所以border的颜色一定不能出现差异

#### 伪元素法
---

利用伪元素可以避免增加不必要的节点，灵活性更进一步。

{% codeblock lang:html %}
<style type="text/css">
img {
  position: relative;
}
img:after {
  content: attr(alt);
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background: #fff; // 背景色遮盖
}
</style>
<img src="error-path" width="100" height="100" alt="no image" >
{% endcodeblock %}

#### 替代法
---
通过**onerror**事件，我们可以为出错的img更换一个备用的src

{% codeblock lang:html %}
<script type="text/javascript">
  function handler(img) {
    img.src="no-image.png";
    img.onerror = null;
  }
</script>
<img src="error-path" alt="no image" onerror="handler">
{% endcodeblock %}