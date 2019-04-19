---
title: http中的状态码和http缓存
date: 2019-04-18 21:25:43
tags: [http]
categories:
  - [http]
description: summary
---

# status code
如果被问到http的状态码，我可以会说出来200，404，500，204。3开头的表示的是重定向，其他的可能就over了。思来想去，还是做一个总结，有的看。

http的状态码中，1xx表示消息，2xx表示成功，3xx表示重定向，4xx表达客户端错误，5xx表示服务器错误。选择几个常见的状态码如下：

### 2xx
码 | 含义
---- | ---
200 | OK，成功处理，有返回内容
204 | No Content，成功处理，没有返回内容

### 3xx
码 | 含义
---- | ---
302 | Found，重定向
304 | Not Modified，资源未修改，不用重新传输（和http cache有关）

### 4xx
码 | 含义
---- | ---
400 | Bad Request，客户端中明显错误，可能的原因：语法错误、无效请求
401 | Unauthorized，未认证，表示当前请求需要用户认证
403 | Forbidden，服务器已经正确理解请求，但拒绝执行。返回信息中应该存在拒绝的原因。
403 | Not Found，请求的资源服务器上找不到

### 5xx
码 | 含义
---- | ---
500 | Internal Server Error，服务器错误
502 | Bad Gateway，网关或者代理的服务器执行请求时，从上游的服务器收到无效请求。

[参考链接](https://zh.wikipedia.org/wiki/HTTP%E7%8A%B6%E6%80%81%E7%A0%81)

# cache
通常情况下请求一个资源，返回的header中可以设置“Cache-Control”来设置缓存策略，浏览器在再次请求时，会先计算当前缓存是否过期，如果过期则去重新请求，如果未过期，则使用现在的缓存。那么这样的方式会有一个问题：当遇到一个紧急修改，这个时候本地的缓存是一个废弃状态怎么办？那么，如何在客户端缓存和快速更新两者之间找一个平衡点？

现在前端使用的自动化工具时webpack，可以去设置打包后的资源名后加上一串hash。服务器可以将缓存时间可以设置成一年甚至更多，当快速更新后，资源的hash改变之后，它对于客户端来说是一个全新的资源，客户端是去请求而不是使用之前的缓存。

[参考链接](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn)


