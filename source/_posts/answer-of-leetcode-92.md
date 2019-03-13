---
title: 一行代码引起的思考
date: 2019-03-12 20:58:38
tags: [算法]
categories: [算法]
description: 这是leetcode的92题的解答
---

leetcode的92题是一道反转从m到n的链表的题目。说起链表，就想起来刚接触链表的时候，随意改变指向的兴奋感。第一门接触的语言是C++，不过想要使用C++去尝试解题的时候，发现我之前接触的已经过时了（用c写链表比js舒服呀），还是拿着js来吧。

回到题目，我在想当m等于1的时候，算不算特殊情况？为了统一来说，还是不当做特殊情况，于是我决定，只拿出m开始的链表进行操作，最后再接上。

下面是我第一次解法：
```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} m
 * @param {number} n
 * @return {ListNode}
 */
var reverseBetween = function (head, m, n) {
  if(m === n) return head;

  let startPrevNode = null;
  let startNode = null;
  let currentNode = head;
  let reverseHeadNode = null;
  let index = 1;

  while(index < m) {
    startPrevNode = currentNode;
    currentNode = currentNode.next;
    index++;
  }
  reverseHeadNode = startNode = currentNode;

  while(index < n) {
    index++;
    currentNode = currentNode.next;

    startNode.next = currentNode.next;
    currentNode.next = reverseHeadNode;
    reverseHeadNode = currentNode;
    // console.log('reverseHeadNode', reverseHeadNode)
    currentNode = startNode;
  }

  if (startPrevNode) {
    startPrevNode.next = reverseHeadNode;
    return head;
  } else {
    return reverseHeadNode;
  }
};
```

答案通过了，但是我的速度只超过了40%的人，内存消耗只少于7%的人，这结果真是沮丧。于是，我开始缕缕算法，发现每次移动的元素都是`startNode`后面的元素，这个时候，我只需要知道`startNode`，而不需要每次保存`currentNode`的值。所以我尝试改了第二个`while`中的操作。

```js
while(index < n) {
  index++;
  currentNode = startNode.next;

  startNode.next = currentNode.next;
  currentNode.next = reverseHeadNode;
  reverseHeadNode = currentNode;
}
```

更改之后，少了一次赋值操作，但是有了很大的提升，速度超了86%， 内存消耗少了15%，算起来效率提升了两倍，算法给人的想象真的是无穷的。我只是删除了一行代码，没想到会有这样的结果。同时最近看的“高性能的javascript”，尽管书出来很多年了，但是其中还是有很多的精髓的地方。

算法使我的code更美，更快，更小！





