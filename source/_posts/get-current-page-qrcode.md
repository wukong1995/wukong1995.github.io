---
title: get-current-page-qrcode
date: 2018-10-31 19:03:08
articleTitle: 获得当前页面的小程序码初探索
tags: ['小程序']
categories: ['小程序']
---

小程序中，获得小程序中的某一篇文章的分享图片，识别图片中的小程序码进去当前文章的详情页， 问题是如何获得当前页面的小程序码呢？

微信给提供了三个接口：
>接口 A: 适用于需要的码数量较少的业务场景
>  生成小程序码，可接受 path 参数较长，生成个数受限，数量限制见 注意事项，请谨慎使用。
>接口 B：适用于需要的码数量极多的业务场景
>  生成小程序码，可接受页面参数较短，生成个数不受限。
>接口 C：适用于需要的码数量较少的业务场景
>  生成二维码，可接受 path 参数较长，生成个数受限，数量限制见 注意事项。

其中，第二个接口的中的其中参数最多接受32个字符。第一和第三的生成个数最多是十万张

#### 步骤
1. 通过小程序的appid和密钥去获得access_token(有效期两个小时)
2. 通过access_token和相应的参数去获得图片的二进制流
3. 将图片的二进制流转换成base64就可以显示了

因为需要canvas将小程序码画出来，调试工具上可以画出来base64的图片，但是在android和iPhone都显示不出来...所以前端去获得小程序是不能完成想要的功能的。和后端沟通的结果是：后端去获得图片返回给前端图片的临时地址...

附上代码：
```js
request(`https://api.weixin.qq.com/cgi-bin/token?grant_type=client_credential&appid=appid&secret=secret`)
  .then(res => {
    request(`https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=${res.data.access_token}`, {
      page: '你的page',
      scene: 'id=2'
    }, 'POST')
      .then(res => {
        // arrayBufferToBase64
      })
  })
```

