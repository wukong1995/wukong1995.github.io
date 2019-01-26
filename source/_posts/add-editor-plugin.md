---
title: 来写一个编辑器的plugin
date: 2019-01-25 19:32:51
tags: [javascript]
categories: [前端, javascript]
description: 在年末的小尾巴，又完成了一个todo
---

今天在纠结是否要更换编辑器，调研了一个新的编辑器，发现功能不能满足需求，也不能增加插件。回过来头，还是在现在的编辑中增加功能吧--写一个plugin。


去看了document和api发现上面的方法不能实现我想要的结果（弹出来的modal）。接着我去看demo，demo中是可以弹出来的modal，我的第一个想法是将代码copy一遍，然后手动的去插入。
于是只能尝试将插件【this】console出来，看一下里面有什么方法。这里还有一个问题，代码是被压缩过的，即使有你想要的方法，参数到底是什么还是需要去猜。技巧是有的，多试试就能知道了。

对于这个plugin，我几个月前是尝试写过的，但是那个时候，并没有写出来，现在看来是方向错了。时间会给人收获和更多的思考，fighting

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1548427982/blog/card_editor_plugin.png)


推荐阅读：(链接)[https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536]，这个作者总共写了6个part，读下来很通顺，读起来没有任何难度（语法很简单的）
