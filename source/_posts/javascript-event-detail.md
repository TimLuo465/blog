---
title: javascript事件流漫谈
date: 2017-05-31 19:30:42
tags:
  - javascript
---
### 先捕获还是先冒泡
---

有这么一个问题：如果为一个节点添加两个事件，一个用捕获，一个用冒泡，哪个先执行，执行几次？

在回答问题之前，先来看看一个完整的DOM事件流案例: <!-- more -->

{% codeblock lang:html %}
<div id="p">
  <p id="c"> hello world </p>
</div>

<script>
    function handler(e) {
        console.log(e.eventPhase);
    }
    
    document.getElementById("p").addEventListener("click", handler, {capture: true});
    document.getElementById("p").addEventListener("click", handler);
    document.getElementById("c").addEventListener("click", handler);
</script>
{% endcodeblock %}

点击"hello world", 结果应该是1,2,3。对应着事件的三个阶段Capture,Target,Bubble。[点这里看完整示例](https://codepen.io/TimLuo465/pen/OmKRdO?editors=0011)

具体过程如下：

![updateClass](/images/event.png)

1. 事件流先由上而下直到目标节点，这一过程所有(不含目标节点)绑定了click事件且capture为true的节点，都将按序触发其handler，这一阶段为捕获阶段(eventPhase=1)。
2. 接着目标节点上绑定的所有click事件根据绑定的先后触发其handler，此为目标阶段(eventPhase=2)。
3. 最后事件流由下而上，从目标节点出发一直到根节点，所有绑定了click事件且capture为false的节点，都将触发其handler，此为冒泡阶段(eventPhase=3)。

看明白了上面的案例，那么问题的答案其实已经呼之欲出了。

### addEventListener(type, callback [, options])
---
前两个参数就不多说了，说说第三个参数options吧。

**options**为可选参数，可以是boolean也可以是object
1. 当不传参时，capture为false，代表着捕获阶段不会触发handler。
2. options为boolean时，capture = options。
3. options为object时，可取如下的属性：

{% codeblock lang:javascript %}
{
    capture: false | true 
    passive: false | true
    once: false | true 
}
{% endcodeblock %}
#### capture

capture为true时，代表着捕获阶段触发handler，其他阶段不触发。这个参数决定着事件是冒泡流还是捕获流，实际情况用冒泡的情况较多，默认为false。

#### passive

 当passive为true时，代表着浏览器将不会调用handler中的**preventDefault**方法，目的是为了提高浏览器的性能。

> 一般情况下用户的大部分输入事件都跟页面元素有关系，一旦页面元素注册了对应事件的监听器，监听器的逻辑代码(handler)必须在内核线程中执行（V8引擎运行在内核线程），因此这种输入事件经常无法立即得到响应。对于一些不经过内核线程，直接由合成线程快速处理的事件(如：手势事件)则响应迅速。

手势输入事件是由连续的普通输入事件组成，这些普通的输入事件可能会在对应的handler中调用preventDefault函数来阻止掉事件的默认行为，这样就需要内核线程的响应，从而无法产生手势输入事件。

passive的作用便是告诉浏览器，这些事件监听器内部不会调用preventDefault函数。这样浏览器将能快速生成手势输入事件，从而让页面响应更快。

[点这里看具体Demo](https://rbyers.github.io/scroll-latency.html)

[点这里了解更多关于passive](http://blog.csdn.net/dj0379/article/details/52883315)

#### once

once为true时，该事件的handler只会被调用一次，结束后将移除对该事件的监听。

### removeEventListener(type, callback [,options])

事件是否能够移除成功，主要取决与type,callback,capture这三个参数，解释器会在节点的事件监听器列表中找到一个type为type, callback为callback, capture为capture的监听器，将其removed属性设置为true，即移除这个事件监听。