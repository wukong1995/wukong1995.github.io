---
title: 安全篇-介绍CSP以及如何应用
date: 2020-11-23 20:03:24
tags: [安全]
categories: [安全]
description: 内容安全策略
---
>内容安全策略   (CSP) 是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本 (XSS) 和数据注入攻击等。无论是数据盗取、网站内容污染还是散发恶意软件，这些攻击都是主要的手段。

你可以配置你的服务器返回 Content-Security-Policy  HTTP头部，也可以使用meta元素配置该策略。

>配置内容安全策略涉及到添加 Content-Security-Policy  HTTP头部到一个页面，并配置相应的值，以控制用户代理（浏览器等）可以为该页面获取哪些资源。比如一个可以上传文件和显示图片页面，应该允许图片来自任何地方，但限制表单的action属性只可以赋值为指定的端点。一个经过恰当设计的内容安全策略应该可以有效的保护页面免受跨站脚本攻击。

#### CSP directives
CSP directives很多，重点只看下面几个（带删除线的都是一般情况下用不着的）:

**1. default-src**
为其他的CSP指令提供备选项，对于以下列出的指令，假如不存在的话，那么用户代理会查找并应用default-src指令的值
* child-src
* connect-src
* font-src
* frame-src
* img-src
* manifest-src
* media-src
* object-src
* script-src
* style-src
* worker-src

**2. script-src**
指定javascript的有效源，不仅包括加载到`<script>`元素中的url，还包括哪联脚本事件处理程序（onclick等）

**3. style-src**
指定stylesheets的有效源

**4. img-src**
指定images和favicons的有效源

**5. connect-src**
用于控制允许通过脚本接口加载的链接地址，受影响的api：
* a标签
* fetch
* xhr
* websoket
* EventSource(https://developer.mozilla.org/en-US/docs/Web/API/EventSource)

**6. font-src**
指定使用@font-face加载字体的有效源

**7. ~~frame-ancestors~~**
指定可以使用`<frame>`，`<iframe>`，`<object>`，`<embed>`或`<applet>`嵌入页面的有效父项。

**8. ~~object-src~~**
指定了 `<object>`, `<embed>`, and `<applet>` 元素的源


**9. media-src**
指定使用audio和video加载的media的有效源


**10. child-src**
它定义了Web Worker的有效源，例如iframe

**11. navigate-to**
会限制文档通过任何方式发起的导航URL，包括form-action、a、window.location、window.open等

#### CSP directives的常用值
**1. `<host-source>`**
域名或者ip地址，网站的地址也可以包含前导通配符，也可以使用`*`作为端口号，表示对所有的合法端口都有效。
例如：`http://*.example.com`，`mail.example.com:443`，`https://store.example.com`，`*.example.com`，

**2. `<scheme-source>`**
像`http:`，`https:`，`data:`，`mediastream`，`blob:`，`filesytem`，

**3. 'self'**
指和现在的来源一样，包括相同的url和端口号。注意你一定要用单引号包起来

**4. 'unsafe-eval'**
允许使用eval（）和类似方法从字符串创建代码。注意你一定要用单引号包起来
2020-12-20补充：**`$('body').html('<script>alert(1)</script>')`这种也属于eval**，[点这里查看](https://stackoverflow.com/questions/37155270/content-security-policy-csp-safe-usage-of-unsafe-eval)，所以你的项目如果是spa一定要加上

**5. 'unsafe-hashes'**
允许启动特定的内联事件处理程序。如果你只需要允许内联事件处理程序，而不允许内联script元素或者`javascript: URL`，与unsafe-inline表达式相比，这是一种更安全的方法。

**6. 'unsafe-inline'**
允许内联资源的使用，例如`<script>`元素、`javascript: URL`、内联的事件处理、内联的`<style>`元素

**7. 'none'**
空集，也就说说没有匹配的url

**8. `'nonce-<base64-value>'`**
使用加密随机数的特定内联脚本的允许列表。服务器应在在每次请求产生唯一的nonce。
```js
<script nonce="xxxxx">
</script>
```

**9. `'<hash-algorithm>-<base64-value>'`**
脚本或样式的sha256，sha384或sha512哈希

#### nodejs的实现
1. 在express中可以使用helmet来实现

```js
const csp = {
  directives = {
    defaultSrc: ["'self'", '*.example.com'],
    scriptSrc: [
      "'self'",
      '*.example.com',
      "'unsafe-inline'",
      "'unsafe-eval'",
      (req, res) => `'nonce-${res.locals.cspNonce}'`,
    ],
    styleSrc: ["'self'", 'blob:', '*.example.com', "'unsafe-inline'"],
    imgSrc: ["'self'", 'data:', '*'],
    connectSrc: ['*', 'data:'],
    fontSrc: ["'self'", '*.example.com', 'data:',],
    mediaSrc: ["'self'", 'data:'],
    childSrc: ["'self'"],
  }
}

app
  .use((req, res, next) => {
    res.locals.cspNonce = uuidV4()
    next()
  })
  .use(helmet.contentSecurityPolicy(csp))
```

2. 在koa中，可以使用`koa-helmet`，代码形式同上


参考资料：
[内容安全策略( CSP )](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)
[default-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)
