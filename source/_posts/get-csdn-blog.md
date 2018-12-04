---
title: 迁移csdn博客
date: 2018-12-04 17:24:47
tags: ['javascript']
categories: ['javascript', '前端']
---

一直想着吧csdn上的文章迁过来，但是一直没动手...这次终于动手了，先想一下我的思路：获取所有的文章链接 -> 获取文章的内容

我使用了：
1. `https`（获取链接的dom）
2. `cheerio`（解析dom）
3. `fs` (向文件中写取内容)

#### 获取页面的html
```js
function getUrlHtml(url, cb) {
  return new Promise(function(resolve, reject) {
    https
      .get(url, function(res) {
        let html = ''
        res
          .on('data', function(data) {
            html += data
          })
          .on('end', function() {
            resolve(cb(html))
          })
      })
      .on('error', function(err) {
        reject(err)
      })
  })
}
```


#### 解析html，并创建promise请求

```js
function getArticleUrl(html) {
  const $ = cheerio.load(html)
  const $articleList = $('.article-item-box.csdn-tracking-statistics h4 a')
  const articleUrlList = []

  $articleList.each(function(index, item) {
    articleUrlList.push($(item).attr('href'))

  })
  return articleUrlList
}
```

#### 执行所有的promise，这里我使用的是`Promise.all`

#### 写入文件
```js
fs.writeFile(`./post/${title}.md`, content, 'utf8', function (error, result) {
  if (error) {
    console.log(error)
    reject(error)
  } else {
    resolve(result)
  }
})
```

#### ps
在获取文章的html内容的时候，获得的是转义后的字符，想着有没有接口可以直接将html转成md？看了csdn上的代码的格式，不知道可不可以转义？

