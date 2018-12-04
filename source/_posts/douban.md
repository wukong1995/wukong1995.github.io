---
title: 豆瓣评分前250部电影数据爬取
date: 2017-08-02 21:32:58
tags: ['nodejs','爬虫']
description: 使用nodejs爬取数据
categories: ['代码']
---
### 在千里码刷题的时候看到了这个题目
题目要求是爬取豆瓣评分最高的250部电影的总分...

#### 分析
爬数据，首先是实用http模块去爬取全部的HTML
然后使用cheerio去得到HTML中自己想要的数据
最后每页得到的总分相加

### 题目中的坑
1、因为豆瓣是https开头，使用https模块，具体的方法没看，使用还是按照http模块来的
2、如果按照正常的逻辑去写代码，最后得到的总分是0，因为https抓取数据是异步进行的
3、使用promise来进行处理，首先需要等到25页的数据全部抓取完毕，再进行计算总分，这时候想到了promise.all这个方法。
4、第一次尝试将使用promise，我在getData中直接将resolve(res)，等到下面使用的时候，res又是一个异步执行，这下尴尬😅了，于是调整顺序，在res执行end事件的时候再resolve
5、js中浮点类型计算的坑，我直接暴力的*10，最后在／10

### 代码
爬评分时，顺便把电影名也爬下来了，我准备把没看的都补上😄

```javascript
const https = require('https')
const cheerio = require('cheerio')
let sumScore = 0
let allMovie = []

function filterMovie(html) {
  let $ = cheerio.load(html)
  let movieList = $('.grid_view li')
  let total = 0
  let movies = []

  movieList.each(function(index, item) {
    let score = $(item).find('.bd .rating_num').text()
    let movieName = $(item).find('.hd a').text().replace(/\s+/g,"")
    movies.push({
      name: movieName,
      score
    })
    total += Number(score) * 10
  })
  return { total, movies }
}

function getData(url) {
  return new Promise(function(resolve, reject) {
    https
      .get(url, function(res) {
        let html = ''
        res
          .on('data', function(data) {
            html += data
          })
          .on('end', function() {
            resolve(filterMovie(html))
          })
      })
      .on('error', function(err) {
        reject(err)
      })
  })
}

let funcArr = []
for(let i = 0; i <= 225; i=i + 25) {
  let url = `https://movie.douban.com/top250?start=${i}`
  funcArr.push(getData(url))
}

Promise
  .all(funcArr)
  .then(res => {
    res.forEach(list =>  {
      const { total, movies } = list
      sumScore += total
      allMovie = allMovie.concat(movies)
    })
    console.log("总分：" + sumScore / 10)
    allMovie.forEach(movie => {
      const { name, score } = movie;
      console.log("评分：" + score + "，影片名：" + name)
    })
  })
  .catch(err => {
    console.log(err)
    console.log("出错了")
  })
```
如果你有更好的想法，欢迎交流👏
