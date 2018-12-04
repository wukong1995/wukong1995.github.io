---
title: 使用node自动发送邮件
date: 2017-11-15 16:08:22
tags: ['nodejs']
categories: ['nodejs']
description: 自动发送邮件：nodemailer
---

#### 发送邮件: [nodemailer](https://github.com/nodemailer/nodemailer)

1. 基本代码，使用之前请确保邮箱开启SMTP

```javascript
const nodemailer = require('nodemailer');

nodemailer.createTestAccount(() => {
  const config = {
    host: 'smtp.163.com',
    port: 465,
    secure: true, // true for 465, false for other ports
    auth: {
      user: 'xxxx@163.com', // generated ethereal user
      pass: 'xxx'  // generated ethereal password
    }
  };

  const transporter = nodemailer.createTransport(config);

  // setup email data with unicode symbols
  const mailOptions = {
    from: '"Fred Foo 👻" <xxx@163.com>', // sender address
    to: 'xxxx@gmail.com', // list of receivers
    subject: 'Hello ✔', // Subject line
    text: 'Hello world?', // plain text body
    html: '<b>Hello world?</b>' // html body
  };

  // send mail with defined transport object
  transporter.sendMail(mailOptions, (error, info) => {
    if (error) {
      return console.log(error);
    }

    console.log('Message sent: %s', info.messageId);
  });
});
```
2. 若使用SSL, 在config中添加

```javascript
secureConnection: true, // use SSL
```
3.添加附件, 在mailOptions添加

```javascript
attachments: [
  {
    filename: '文档.txt', // 不会乱码
    content: '哈哈哈'
  },
  {
    filename: '2.txt',
    content: 'heool word'
  }
]
```
4. 添加图片, 在mailOptions添加

```javascript
attachments: [
  {
    filename: '文档.txt',
    content: '哈哈哈'
  },
  {
    filename: '01.png',   // image
    path: './flow.png',   // 图片路径
    cid: '00000001'
  }
]
```

