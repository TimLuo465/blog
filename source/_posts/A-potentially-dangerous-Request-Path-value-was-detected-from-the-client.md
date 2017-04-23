---
title: A potentially dangerous Request.Path value was detected from the client
tags:
  - 技术文章
date: 2016-04-10 19:47:31
---

> **Sernario**

在asp.net 4.0项目，URL中含有<span style="color: #808000;">&lt;,&gt;,*,%,&amp;,:,/<span style="color: #000000;">时，页面会抛出apllication error，A potentially dangerous Request.Path value was detected from the client<span style="color: #ff0000;">异常<span style="color: #000000;">。</span></span></span></span><!--more-->

> **Reason**

asp.net默认的拦截机制，其目的是为了确保URL传输的安全。

> **Solution**

**1. requestPathInvalidCharacters**

` <system.web>
    <httpRuntime requestPathInvalidCharacters="" />
 <system.web>`

默认都不拦截(&lt;,&gt;,*,%,&amp;,:,/七种特殊字符)，如果需要拦截直接写在""内，以逗号隔开（&amp;lt;,&amp;gt;,*,%,:,&amp;amp;,/）。