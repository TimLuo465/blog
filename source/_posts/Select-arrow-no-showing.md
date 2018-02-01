---
title:  如何隐藏select框的下拉箭头
date: 2017-06-11 20:13:37
tags: 
    - css
---

#### 解决方案

{% codeblock lang:css %}
select {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    /* background-image: url(...);  自定义下拉箭头 */
}
/* IE10+ */
select::-ms-expand {
    display:none;
}
/* IE9 */
@media screen and (min-width:0\0) {
    select {
        background:none\9;
        padding: 5px\9;
    }
}
{% endcodeblock %}

具体效果[查看示例](https://codepen.io/TimLuo465/pen/NyqmOV)

#### 参考链接
1. [how-to-style-a-select-dropdown-with-css-only-without-javascript](https://stackoverflow.com/questions/1895476/how-to-style-a-select-dropdown-with-css-only-without-javascript)