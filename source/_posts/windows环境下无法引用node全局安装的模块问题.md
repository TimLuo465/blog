---
title: windows环境下无法引用node全局安装的模块问题
tags:
  - node
  - 技术文章
date: 2016-05-17 21:55:12
---

#### **问题**

在node项目中，往往需要安装一些依赖的包，通常我们采取全局安装的方式，来减少一些包重复安装带来的烦恼。<!--more-->

但是全局安装后出现无法使用的情况，可能是你NODE_PATH没有设置或者不正确造成的。

#### **解决方案**

那么，什么是NODE_PATH呢？

NODE_PATH是node<span style="font-size: 14px; color: #808000;">为模块提供寻找路径的一个环境变量<span style="color: #000000; font-size: 16px;">。</span></span><span style="font-size: 14px;"><span style="font-size: 16px;">关于node模块加载策略，</span></span><span style="font-size: 14px; color: #808000;"><span style="color: #000000; font-size: 16px;">可以参考[这里](http://www.infoq.com/cn/articles/nodejs-module-mechanism/)。</span></span>

那么，如何配置NODE_PATH呢？

很简单，只需要在环境变量中新添加一个名为NODE_PATH的变量，值为<span style="color: #808000; font-size: 14px;">**npm**<span style="color: #000000; font-size: 16px;">的安装目录，例如:</span></span>

` C:\Users\XX\AppData\Roaming\npm\node_modules `

&nbsp;

![node path config](/images/NODE_PATH_CONFIG.png)