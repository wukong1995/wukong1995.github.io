---
title: 如何用redis实现锁
date: 2020-06-15 14:21:34
tags: [redis]
categories: [后端, redis]
description: 加上锁，想怎么办就怎么办
---

今天遇到一个问题是，我正准备调用api的时候，突然来了一个取消的信号，这一瞬间，就导致调用api成功，取消也成功了。但是实际上真正想取消的没有取消。这个时候需要一个锁，来固定一下。使用的是redis来进加锁。

#### 1.使用setnx来做
setnx的意思是set if not exist,如果set成功就反悔字符串“ok”，不成功返回null

```js
client1.setnx(key, value) // 成功
client2.setnx(key, value) // 失败
client1.del(key)
client2.setnx(key, value) // 成功
```

利用setnx的属性，成功了就说明抢到锁了；进行操作后，将锁删掉后，别人抢到锁后才能进行操作

剩下的方法待补充...