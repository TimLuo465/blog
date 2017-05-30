---
title: vue之可怕的class绑定
date: 2017-05-28 20:02:19
tags:
  - javascript
  - vue
---
#### class绑定
---
在vue中可以利用v-bind:class(即 **:class**)来动态切换class，具体用法就不多赘述了，[官方文档](https://vuefe.cn/v2/guide/class-and-style.html)很齐全。<!-- more -->

#### 可怕之处
---
当你用错误的方式打开 **:class** 之后，你会怀疑你的人生。废话不多说，看看下面的例子

{% codeblock lang:html %}
<template>
<div id="classBind" :class="{active: isActive}" @click="toggleActive"></div>
</template>

<script>
new Vue({
    el: "#classBind",
    data: {
        isActive: false
    },
    methods: {
        toggleActive: function() {
            this.isActive = !this.isActive;    
        }
    },
    mounted: function() {
        // 动态添加class
        this.$el.classList = "border-red"
    }
});
</script>

<style>
div {
    border: 1px solid #333;
    width: 50px;
    height: 50px;
}
.active {
    background: #ccc;
}
.border-red {
    border: 1px solid red;
}
</style>
{% endcodeblock %}
咋一看这样子好像没毛病，结果确实点击div后active切换了，border变回了黑色，说好的红色去哪了？[点这里看具体的效果](https://codepen.io/TimLuo465/pen/GmaOYq?editors=0010)。

接下来，再看一个常见的场景，比如我们需要使用某个滚动条插件来对滚动条进行美化，在某个节点上进行了初始化，但恰巧我们又在该节点上使用了 **:class** ，事实告诉你，你可能用了一个假插件。[来看看究竟会发生什么](http://note.youdao.com/)。

看过这两个例子，机智的你会发现动态添加在节点的类不见了，Why?

#### :class都做了什么
---
以vue2.3.3为例，在源码中找到updateClass这个方法，如下图所示

![updateClass](/images/updateClass.png)

这里就是update class的主要方法。

**5579行**的cls就是:class绑定的所有class的值，如

{% codeblock lang:html %}
:class={active: isActive, highlight: isHightLight}
{% endcodeblock %}
isActive, isHightLight为ture,对应的cls就是"active hightlight"。反之则为""。

**5589行**则是将节点的class属性进行重新赋值，更新。这也就是插件动态添加的class消失的原因了。可以自行debug一下，更清晰一些。

#### 总结
---
原谅我只是个标题党，可怕的不是:class而是用错:class的人。谨以此提醒一下，不要在节点上即使用了:class又初始化了需要添加特定class的插件。不然死都不知道死的，你懂的。



