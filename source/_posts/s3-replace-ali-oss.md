---
title: 使用s3协议上传ali oss
date: 2019-08-10 23:39:03
tags: [后端]
categories: [后端]
description: simple storage service
---

### 前言
最近要做的是将静态资源放在服务器上，目前项目的文件存储阿里的oss上，考虑业务和后续发展，以后会有更多的云供选择，所以找一个通用的协议来做这些上传。

问了后端的同学，ali oss兼容aws的上s3协议，但是s3不兼容ali oss，所以选用s3协议来上传ali oss。

### s3是什么
全称是Amazon Simple Storage Service
[介绍在这里](https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/dev/Introduction.html)

### 配置项目使用oss
这里我用到的上传打包后的文件到oss的一个webpack的plugin是[webpack-aliyun-oss](https://github.com/gp5251/webpack-aliyun-oss)

1. 配置打包时文件的publicPath
2. 配置打包后上传文件
```js
new WebpackAliyunOss({
    dist: 'dist/',
    region: 'oss-region',
    accessKeyId: 'accessKeyId',
    accessKeySecret: 'accessKeySecret',
    bucket: 'bucket'
})
```
配置完后，测试一下没有问题即可。

### 使用s3上传oss
使用[aws-sdk](https://github.com/aws/aws-sdk-js)来进行上传，说到这里，我去网上找demo，找到的一个java的例子，怎么到了js这里配置咋就这么麻烦呢？

s3有好几种signatureVersion，本来想这个一个一个的尝试，试到v2的时候，总算是不报signature不匹配的问题了，幸亏问了一下后端的同学，才知道oss用的version时v4，完成配置后就可以正常上传了。


### 使用s3上传minio
本来时改配置的事，但是会抱一个未知的inaccesshost的错误，直接找的同学解决了一下，s3将bucket放在endpoint中有两种方式：
1. bucket.host.com
2. host.com/bucket

使用s3ForcePathStyle设置，默认false，展现形式是第一种，设置成true就是第二种。


### 遗留问题
1. 使用s3协议上传的图片，复制链接在浏览器打开只能下载，不能预览，这个是否可以通过设置“Content-type”来解决？
2. 做成一个webpack的plugin？

### 2020.02.06补充
1. 上传时指定Content-type

```js
// 设置参数
AWS.config.setPromisesDependency(Promise)
AWS.config.update({
  region,
  accessKeyId,
  secretAccessKey,
  endpoint,
  httpOptions: {
    timeout
  }
})

// 实例化s3
const s3 = new AWS.S3({
  signatureVersion = 'v4',
  Bucket: bucket,
  s3ForcePathStyle
})

// 上传文件
const result = yield s3
    .putObject({
      Key: fileName,
      Bucket: bucket,
      Body: fs.readFileSync(filePath),
      ContentType: mime.lookup(fileName)
    })
    .promise()
```

2. 参照[webpack-aliyun-oss](https://github.com/gp5251/webpack-aliyun-oss)，做出来一个webpack plugin.
