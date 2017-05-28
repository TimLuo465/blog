---
title: LeetCode - Two Sum
tags:
  - javascript
category:
  - LeetCode
date: 2017-03-08 00:27:32
---

#### Question:

<span style="font-size: 14px;">Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

 </span><span style="font-size: 14px;">You may assume that each input would have **_exactly_** one solution, and you may not use the _same_ element twice.</span> <!--more-->

<span style="font-size: 14px;">**Example:**

 </span>

<pre>Given nums = [2, 7, 11, 15], target = 9,
Because nums[**0**] + nums[**1**] = 2 + 7 = 9,
return [**0**, **1**].</pre>

<span style="font-size: 14px;">**Solution:**</span>

题目大概意思是从一个整数数组中，找出两个数之和等于给定的数，并返回这两个数的下标。

题目很简单，暴力点两个for循环就能解决问题，时间复杂度O(n<sub>2</sub>)

{% codeblock lang:javascript %}
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var l = nums.length, x, y;

    for(x = 0; x < l - 1; x++) {
        for(y = x + 1; y <= l - 1; y++) {
            if(nums[x] + nums[y] == target) {
                return [x, y];
            }
        }
    }
};
{% endcodeblock %}

仔细的想想的话, 还是能降到O(n)的

{% codeblock lang:javascript %}
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var l = nums.length,
        arr = [], i, j;

    for(i = 0; i < l; i++) {
        j = arr.indexOf(target - nums[i]);

        if( j != -1) return [i, j];

        arr.push(nums[i]);
    }
};
{% endcodeblock %}

虽然复杂度降下来了，但是代码还能继续优化一下，用对象的方式来取代数组的indexOf，这样性能更佳

{% codeblock lang:javascript %}
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    var o = {}, j, i, num;

    for(i = nums.length; i--;) {
        num = nums[i];
        j = o[target - num];

        if( j > -1) return [i, j];

        o[num] = i;
    }
};
{% endcodeblock %}