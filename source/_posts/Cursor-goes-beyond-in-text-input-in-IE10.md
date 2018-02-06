---
title: 'IE10+中input框光标越界问题'
date: 2017-08-01 11:31:15
tags: 
    - ie issues
    - css
---

#### 问题概要
ie10中, 动态给input框赋值(值的长度超过input宽度)时, input的光标会focus在input的最边缘

![cursor-beyond-in-input](/images/cursor-beyond-in-input.png)

<!--more-->

#### 解决方案

主要通过::ms-clear和::-ms-value来解决该问题，具体的效果实现需要自行尝试。

{% codeblock lang:css %}
@media screen and (-ms-high-contrast: active), (-ms-high-contrast: none) {
    input[type="text"]::-ms-value {
        margin-right:10px;
    }
    input::-ms-clear {
      display: none;
    }
}
{% endcodeblock %}

#### 参考链接
1. [cursor-goes-beyond-in-text-input-in-ie10](https://stackoverflow.com/questions/25402513/cursor-goes-beyond-in-text-input-in-ie10)
2. [html-ie-right-padding-in-input-text-box](https://stackoverflow.com/questions/3765411/html-ie-right-padding-in-input-text-box)