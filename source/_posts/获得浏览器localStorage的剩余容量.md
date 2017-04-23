---
title: 获得浏览器localStorage的剩余容量
tags:
  - Javascript
  - 技术文章
date: 2016-03-20 13:27:44
---

### 一、如何获取localStorage的剩余容量

在H5大行其道的今天，localStorage(本地存储)对每一个前断攻城师来说都不太陌生。同时localStorage也给我们带来了极大的便利，不用于cookies的小家子气，localStorage的存储量还是很客观的。<!--more-->

由于浏览器的差异性，导致我们需要对localStorage中的内容做出一部分取舍。而根据目前的市场上浏览器对loaclStorage的兼容性来看，最佳的大小是<span style="color: #ff0000;">2.5M</span>左右。如何得到localStorage的容量大小，大概接触过localStorage都会遇到的这么一个问题。

**localStorage **中存储的是字符串，根据这一条件，我们可以通过取出所有的localStorage的内容，而其长度就是大小。具体代码如下:
`(function(){
     if(!window.localStorage) {
         console.log('浏览器不支持localStorage');
     }
     var size = 0;
     for(item in window.localStorage) {
         if(window.localStorage.hasOwnProperty(item)) {
             size += window.localStorage.getItem(item).length;
         }
     }
     console.log('当前localStorage剩余容量为' + (size / 1024).toFixed(2) + 'KB');
 })()`

### 二、如何获取localStorage最大容量

通过上面的分析，其实思路基本是一样的，都是通过字符长度来判断。废话不多说，贴码:
` (function() {
    if(!window.localStorage) {
    console.log('当前浏览器不支持localStorage!')
    }    var test = '0123456789';
    var add = function(num) {
      num += num;
      if(num.length == 10240) {
        test = num;
        return;
      }
      add(num);
    }
    add(test);
    var sum = test;
    var show = setInterval(function(){
       sum += test;
       try {
        window.localStorage.removeItem('test');
        window.localStorage.setItem('test', sum);
        console.log(sum.length / 1024 + 'KB');
       } catch(e) {
        console.log(sum.length / 1024 + 'KB超出最大限制');
        clearInterval(show);
       }
    }, 0.1)
  })()`

自己写了一个[检测浏览器localStorage的最大容量](http://www.frontend.ren/localstorage/ "测试浏览器localStorage的最大容量")的页面，麻雀虽小，但五脏俱全 :lol:。