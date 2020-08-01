---
title: 怎样做一个安全的token
date: 2020-08-01 13:42:48
tags: [前端]
categories: [前端, react]
description: 原则：如果密钥泄漏了怎么办
---

周五分享的csrf新get到的点，csrf安全的前提一定是没有xss攻击，否则你做的防御根本起不到作用。
参考文章[https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)

csrf攻击过程，你已经在网站A上登录了，这个时候就有了A网站下cookie；当你浏览B网站时，B网站有一个向A网站提交的表单，网站会诱导你去点击，点击后，就立即向A发送了一个请求，这个时候浏览器会自动携带A网站下的cookie，A网站在验证你的cookie后，就立即执行请求内容。于是，在用户不知情的情况下，可能操作了危险的操作，例如转账等...

分享过程中，提到了token，token可以和cookie配合使用，用来做双重认证。比较熟悉的就是小程序中的JWT。那么怎么做才能保证token足够安全呢？

token使用原则：理论上一个token只能用一次，当你使用了token，服务器会再给你下发一个token，但是这样做的代价大，所以大部分时间用户登录后，会拿到一个token，保存本地，发请求的时候再带过去。

1. 不使用redis，直接使用算法加密
现在正在做的项目，后端同学直接在用户的id基础上进行加密，然后下发一个token给前端，但是这样带来的一个问题是，万一加密的密钥被获取到，因为用户的id是number类型，是可以枚举的，那么攻击者很有可能会去枚举用户id，进而获得每个用户的内容。如果发现泄漏，第一想到的解决办法是修改密钥，修改后，会导致全部的用户需要重新登录，嗯，这应该是一个事故...

2. 在1的基础上，token后面增加随机字符串
并且随机字符串增加过期时间，这就杜绝了即使攻击者去枚举用户的id得到用户的信息。一定不要将随机字符串放在数据库中，否则大量肉鸡就会将数据库拖垮。可以放在redis中，利用redis天然的过期特性。

3. 在2的基础上，我加入多个密钥
一个token经过多重加密，即使泄露了一个也没关系。


其他：
1. 还有一个有趣的，微信抢红包，微信会给你下发一个token，前端收到token后，发送给后端，后端将token推到消息队列中，慢慢消费...

PS: 去朋友家做客，体验了一把手机投屏，投完屏之后，我还可以在手机上看别的，感觉特别神奇，找了找相关资料，感兴趣[点这里](https://www.zhihu.com/question/287361675)


