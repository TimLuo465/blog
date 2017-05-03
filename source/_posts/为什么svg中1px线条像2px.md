---
title: 为什么svg中1px线条像2px
date: 2017-05-03 09:41:57
tags:
---
#### 1px的线变2px

---

当我们使用svg来画线时, 将stroke-width设置为1。理想情况下,下面的代码应该是生成一条1px宽的水平直线。

{% codeblock lang:html %}
<svg style="width:100%;">
    <line y2="10" y1="10" x2="100%" x1="10" stroke-width=1 stroke=#111 />
</svg>
{% endcodeblock %}

但是结果却是一条2px宽的直线摆在眼前。
<!--more-->
#### 为什么

---

导致这一现象的原因是由于[抗锯齿](http://baike.baidu.com/link?url=Q84kci79JfLL2OqPJPluyu_nQ4MkQf9S0k5gsy9stFEeLnSYvxRPfQUtBCTir9v9Ukm6KUOcGD-_LY5ngbYFK7EfD7PSjwbzNx9juxmPekRNkz_o0Ya_gNuJoyjsB1W5)的存在，当坐标为整数时，抗锯齿会与坐标像素重合，直线周围出现与stroke相近的模糊色，使得其看起来像是2px。

> 浏览器会自动权衡渲染速度、平滑度、精确度。默认是倾向于精确度而非平滑度和速度。也就是说浏览器通常是默认开启抗锯齿的。

#### 如何关闭抗锯齿

---

  **shape-rendering**
  
  可以指定svg的渲染模式。可取如下四个值:
  
- auto
  
  让浏览器自动权衡渲染速度、平滑度、精确度。默认是倾向于精确度而非平滑度和速度。

- optimizeSpeed

  偏向渲染速度，浏览器会关闭抗锯齿模式。（速度）

- crispEdges

  偏向更加清晰锐利的边缘的渲染。浏览器在渲染的时候会关闭抗锯齿模式，且会让线条的位置和宽度和显示器边缘对齐。（锐度）

- geometricPrecision

  偏向渲染平滑的曲线。（平滑）

#### 关闭抗锯齿可能存在的问题

---

关闭抗锯齿可以使直线清晰，但如果是圆形呢，则会使得圆形出现毛边。

所以使用shape-rendering的时候需要尽可能缩小使用范围，避免相互间的影响。

#### 另一种解决方案

---

将坐标加0.5，同样可以解决这个问题。

{% codeblock lang:html %}
<svg style="width:100%;">
    <line y2="97.5" y1="97.5" x2="100%" x1="10" stroke-width=1 stroke=#111 />
</svg>
{% endcodeblock %}

#### 具体例子

---

{% jsfiddle gth0pc7v html,css,result light 100% 320px %}
