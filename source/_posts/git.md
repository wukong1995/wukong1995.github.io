---
title: 使用git的那些年
date: 2017-07-22 22:38:02
tags: [Git]
description: 这个主要是分享使用git的那些事
categories: [Git]
---

### 使用git想要去合并两个commit

今天好不容易完成了工作，睡觉的时候突然想到还有些小瑕疵，就想改正。但是改动范围很小，不好意思在commit一次，于是就想将两次commit合并在一起，但是我没有将最新的commit推送到远程分支，就直接合并了两个commit成了一个新的commit，在推送到远程时，也没有先进行pull的动作（👉每次push之前一定要进行pull的动作），导致远程commit和我合并的commit发生冲突，最后还需要手动merge。看了一下提交记录，吓坏了，本来我想合并commit，预计的commit只会在之前的基础上多一次commit，但是现在多了三次commit，这下坏了。
于是，上网查询，试了好几种办法。找到了一个很合适的办法
```
git  reset HEAD^ --hard    //彻底回退到上次提交
git push origin 分支名 -f   // 强制推送到远程
```
这样操作以后，我的问题解决了

待续...

-补充---
最好不要不要强行push😕
