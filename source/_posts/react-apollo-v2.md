---
title: 升级react-apollo v2 踩坑
date: 2020-07-11 21:49:40
tags: [graphql]
categories: [前端, graphql]
description: throttle and debounce
---

拖到现在才升级react-apollo，一直用的是v1，随着react的升级，是真的害怕会阻碍react的升级，于是这个Q决定升级它。

migrate V2文档[点这里](https://www.apollographql.com/docs/react/v2.5/recipes/2.0-migration/)，跟着文档升级还是挺快速的。替换完成后，启动发现可以正常运行。但是目前server端会有返回401、403、405的状态，需要验证一下

### 问题出现
根据文档上来，处理4xx的状态这样来：
```js
const errorLink = onError(({ networkError = {}, graphQLErrors }) => {
  if (networkError.statusCode === 401) {
    logout();
  }
})
```

但是我在项目中，收到的networkError一直是字符串`...failed to fetch`(大概是这个意思)，怎么也收不到statusCode这个状态码，于是去找到相关联的issue，还真有这个问题，[issue300](https://github.com/apollographql/apollo-link/issues/300)，issue中给的解决方法是调整networkError的ts类型，虽然刚开始比较疑惑数据为什么和类型有关，但是还是抱着希望试了试，发现并没有什么用😭。随即一直在issue中遨游，但是还是没有找到解决办法

### 问题·查
没办法，只能一路去源码里面找了，看代码，没看出来啥问题，但就是不行。只能在怀疑的地方加断点debugger了，最后查到了是apollo-link-http-common中parseAndCheckHttpResponse函数的问题
```js
var parseAndCheckHttpResponse = function (operations) { return function (response) {
    return (response
        .text()
        .then(function (bodyText) {
        try {
            return JSON.parse(bodyText);
        }
        catch (err) {
            var parseError = err;
            parseError.name = 'ServerParseError';
            parseError.response = response;
            parseError.statusCode = response.status;
            parseError.bodyText = bodyText;
            return Promise.reject(parseError);
        }
    })
        .then(function (result) {
        if (response.status >= 300) {
            throwServerError(response, result, "Response not successful: Received status code " + response.status);
        }
        if (!Array.isArray(result) &&
            !result.hasOwnProperty('data') &&
            !result.hasOwnProperty('errors')) {
            throwServerError(response, result, "Server response was missing for query '" + (Array.isArray(operations)
                ? operations.map(function (op) { return op.operationName; })
                : operations.operationName) + "'.");
        }
        return result;
    }));
}; };
```

能看出来上面的代码有问题吗？是reponse.text()的时候出error了，这里抛出的error是一个string，导致走不到下面的then去判断status。

### 问题·查之reponse.text()
react-apollo的fetch用的是浏览器原生的fetch，记得之前用fetch咋写吗
```js
fetch(url)
  .then(response => response.json())
  .then(res => {
    // 处理数据
  })
```
.json方法是针对于server返回的json数据类型处理的，.text就是针对text/xxx处理。在[stackoverflow](https://stackoverflow.com/questions/56227336/how-to-handle-response-json-and-text-using-fetch)上看了一个回复，加上我之前在issue上说可能是服务端返回的数据的问题，让我把目光转向了server。

首先200是可以的，是不是401的时候不能text吗？先找资料，没有，那可能是错的。写代码试试

```js
var http = require(‘http’);
var server = http.createServer(function (request, response) {
    console.log(request.method + ‘: ‘ + request.url);
    response.writeHead(401, {‘Content-Type’: ‘text/plain’, ‘Access-Control-Allow-Origin’: ‘*’});
    response.end('Hello world!');
});

// 让服务器监听65534端口:
server.listen(65534);

console.log(‘Server is running at http://127.0.0.1:65534/');
```

在chrome的控制台上用fetch，`response.text()`是可以的。那问题出在哪呢？
在server的代码中我看到写的是`res.end();`，我尝试在end中加上`hello world`，结果可以了！

关于text()的解释是：
>The text() method of the Body mixin takes a Response stream and reads it to completion. It returns a promise that resolves with a USVString object (text). The response is always decoded using UTF-8.

难道是因为没有流导致的报错？这个时候我想起来了一个状态码204，它表示no content，就是没有任何返回数据，那对于这个请求，按照常规的fetch写法是不是也会进入catch中，结果真的在fetch的[issue](https://github.com/whatwg/fetch/issues/113)中找到了这个问题，里面说的解决方案是：

```js
const r = await fetch('https:...', { method: 'POST', .. });
if (r.ok) {
  const data = await r.json().catch(() => null);
  // ...
}
```
node-fecth也有类似的的讨论: [https://github.com/node-fetch/node-fetch/issues/165#issuecomment-258718522](https://github.com/node-fetch/node-fetch/issues/165#issuecomment-258718522)，里面的意思是不太符合规范。额，这个...我还是很喜欢204这个状态的...


