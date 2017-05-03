---
title: LeetCode - Add Two Numbers
tags:
  - LeetCode
  - javascript
date: 2017-03-09 01:18:42
---

**Question:**

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself. <!--more-->

<span style="font-size: 14px;">**Example:**

 </span>

<pre>**Input:** (2 -&gt; 4 -&gt; 3) + (5 -&gt; 6 -&gt; 4)
**Output:** 7 -&gt; 0 -&gt; 8
</pre>

<span style="font-size: 14px;">**Solution:**</span>

给定两个非空链表表示两个非负整数，数字以倒序的方式存储，且每个节点包含一个数字，两数相加返回一个新的链表

假定两个数的第一位不为零，除其本身为零外。

根据题目给出的前置条件ListNode对象，可以看出

{% codeblock lang:javascript %}
//[2,4,3]等价于
new function(){ 
    this.val = 2;
    this.next = new function() {
        this.val = 4;
        this.next = function() {
            this.val = 3;
            this.next = null;
        }
    }
}
{% endcodeblock %}

有几个地方需要注意的是
1\. 两个链表长度可能不同
2\. 满10需要进位
3\. 返回的结果也需要是ListNode型

利用递归解题

{% codeblock lang:javascript %}
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2, carry) {
    var list,sum;

    sum = l1.val + l2.val + (carry || 0)
    list = new ListNode(sum % 10);
    carry = sum /10 >>> 0;

    if(l1.next || l2.next || carry == 1 ) {
        list.next = addTwoNumbers(l1.next || new ListNode(0),
            l2.next || new ListNode(0), carry);
    }
    return list;
};
{% endcodeblock %}

不重新创建新的对象，使用原有的对象

{% codeblock lang:javascript %}
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function(l1, l2) {
    var sum;
    var itera = function (l1, l2, carry) {
        if(!l1 && !l2 && !carry) return;

        sum = l1.val + l2.val + carry;
        l1.val = sum % 10 ;
        carry = sum / 10 >>> 0;

        if(!l1.next && (l2.next || carry)) {
            l1.next = new ListNode(0);
        }

        if(!l2.next && (l1.next || carry)) {
            l2.next = new ListNode(0);
        }

        return itera(l1.next, l2.next, carry);
    }

    itera(l1, l2, 0);

    return l1;
};
{% endcodeblock %}