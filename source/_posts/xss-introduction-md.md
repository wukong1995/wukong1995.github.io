---
title: 安全篇-介绍xss以及如何应用
date: 2020-12-11 16:36:36
tags: [安全]
categories: [安全]
description: xss-跨站脚本攻击
---

### xss的类型
1. 反射型
例如：从url中读取再渲染，在项目中，读取出来的值会当作string去渲染，不会使用innerHTML

2. 存储型
例如：知识点的答案，富文本中输入script标签，这个会在node过滤掉，但是对于<a href="javasctipt: xxx">，这个前端在渲染的时候会使用xssFilter过滤掉，即使不过滤，在增加csp的情况下，也执行不了。

```js
import { filterXSS } from 'xss'

export function xssFilter(html: string) {
  return filterXSS(html)
}
```

3. DOM型
例如：一般在搜索列表的时候，返回的数据会在原来的string的基础上，加上em标签，这个时候你不得不使用innerHTML来使em标签起作用，假如title是`<img src=1 onerror='xxx'>`，这个就中枪了


#### 策略
1. [**high**]重要对外的项目一定增加csp策略，例如吾来、mage、官网

2. [**high**]第三方组件/资源
- 禁止引入非可信来源的第三方js，应检查页面内引入的第三方js资源是否可控

3. [**high**]禁止使用[eval](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)

4. [**high**]开启xss保护模式："X-XSS-Protection": "1; mode=block"
- [X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection)：浏览器监测到xss的攻击会立即阻止页面的加载。浏览器支持性不太好
```js
app.use(helmet.xssFilter());
```

5. [**middle**]定期检查依赖关系
- 使用npm/yarn audit得到易受攻击的列表，升级他们来避免安全问题，github中有专门的bot来检测repo的依赖，有风险的会自动提pr

6. [**middle**]减少没有必要的dangerouslySetInnerHTML使用

7. [**middle**]设置[Referrer-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referrer-Policy): 用来控制在request中可以包换多少refer信息
设置方式：
- 在header中设置："Referrer-Policy": "no-referrer"
```js
app.use(
  helmet.referrerPolicy({
    policy: ["origin", "unsafe-url"],
  })
);
```

- 或者a标签增加rel = noopener or noreferrer
- meta中设置

8. [**low**]使用script标签引入资源时，增加integrity来验证资源没有被篡改过
```js
<script src= "https://example.com/example-framework.js" integrity= "sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/ux..." crossorigin= "anonymous" ></script>
```

9. [**low**]增加Strict-Transport-Security、X-Frame-Options、X-Content-Type-Options相关头部设置
- Strict-Transport-Security: 允许网站告诉浏览器应该使用https访问，而不是http
- X-Frame-Options： HTTP响应标头可用于指示是否应允许浏览器在<frame>，<iframe>，<embed>或<object>中呈现页面
- X-Content-Type-Options: 相当于提示标志，被服务器用来提示客户端一定要遵循在content-type中对MIME类型的设定

10. [**low**]Feature-Policy: 限制浏览器特性和api的访问(该功能还在实验状态)
```
"Feature-Policy": "accelerometer 'none'; ambient-light-sensor 'none'; autoplay 'none'; camera 'none'; encrypted-media 'none'; fullscreen 'self'; geolocation 'none'; gyroscope 'none'; magnetometer 'none'; microphone 'none'; midi 'none'; payment 'none';  picture-in-picture 'none'; speaker 'none'; sync-xhr 'none'; usb 'none'; vr 'none';"
```

// 可以在这个网站[securityheaders](https://securityheaders.com)得到网站的分数

参考链接：
* [面向DevSecOps的编码安全指南｜JavaScript篇](https://mp.weixin.qq.com/s?__biz=MjM5NzE1NjA0MQ==&mid=2651203073&idx=1&sn=75a74a7828fa82aba102c27c168013a0&chksm=bd2cc3a78a5b4ab19bdb65586f99dc265a4ee17daab352099ca094c4866db73f9d89c28a9c60&mpshare=1&scene=1&srcid=1127mDcLh2pDQvJSWUOKh5FB&sharer_sharetime=1606990270159&sharer_shareid=6e04a8b1ba373e3353f8c40f9273cb97&version=3.0.28.2286&platform=mac#rd)
* [13 Security Tips for Front-End Apps](https://medium.com/better-programming/frontend-app-security-439797f57892)
* [Prevent DOM-based cross-site scripting vulnerabilities with Trusted Types](https://web.dev/trusted-types/)
* [Protect the frontend](https://andrepolischuk.com/protect-the-frontend/)
* [Does Security Matter to Front End Developers and Tips To Not Get Hacked](https://datahouse.asia/security-tips-front-end-developers/)
* [10 security tips for frontend developers](https://konstantinlebedev.com/security-for-frontend/)
* [Cross-site_scripting mdn](https://developer.mozilla.org/zh-CN/docs/Glossary/Cross-site_scripting)
* [前端安全系列（一）：如何防止XSS攻击？](https://segmentfault.com/a/1190000016551188)
* [Cross-site scripting](https://portswigger.net/web-security/cross-site-scripting)



