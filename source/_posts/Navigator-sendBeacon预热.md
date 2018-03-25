---
title: Navigator.sendBeacon预热
tags:
  - javascript
date: 2018-03-25 14:26:03
---

#### 概要
当用户关闭网站或者应用时，我们经常有需求是将一些数据(如: 日志信息)传回服务器端。面对这种情况我们可能会这么做:

```
window.onunload = function () {
    // TO DO async ajax
}
```

那么问题来了，由于浏览器通常会在卸载事件中忽略handler中的异步请求。所以请求最终还是无法发出。

既然异步会被忽略，那么只能用同步请求来发送请求了。虽然同步请求可以确保请求发送出去，但是关闭页面时会有明显的延迟卡顿，带来糟糕的用户体验。

<!--more-->

#### sendBeacon登场

``sendBeacon``的出现，很简单的就解决了上述这类场景。

使用``sendBeacon``可以在页面卸载事件中，通过异步请求将数据传回后台，同时不影响用户体验。

```
window.onunload = function () {
    navigator.sendBeacon(url, data);
}
```

#### 用法和细节

```
navigator.sendBeacon(url [, data]);
```

```data``` 参数可选，可以是 ```ArrayBufferView```, ```Blob```, ```DOMString```, 或者 ```FormData``` 类型的数据。

使用```sendBeacon```需要注意:
1. ```sendBeacon```只能发送POST请求
2. ```sendBeacon```有返回值，```true```表示浏览器成功地将需要发送的数据放入处理队列中。```false```则没有，意味着请求无法发送成功。

#### 兼容性

```sendBeacon```目前处于实验阶段，chrome，firefox，Edge有良好的支持，ie，Safari目前并不支持。

#### 相关链接

-  [W3C - Beacon](https://w3c.github.io/beacon/)