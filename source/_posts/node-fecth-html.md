---
  date: 2016-05-29 13:15:32
  title: nodejs抓取网页的源码，并保存到本地文件
  tags: [nodejs]
  categories: [nodejs]
  description:
---

```js
var http = require('http')
var fs = require('fs');

// 要抓取的网页地址
var url = 'http://www.imooc.com/learn/348'

http.get(url, function(res) {
	var html = ''
	res.on('data', function(data) {
		html += data;
	})
	res.on('end', function() {
		// 将抓取的内容保存到本地文件中
		fs.writeFile('index.html', html, function(err) {
			if (err) {
				console.log('出现错误!')
			}
			console.log('已输出至index.html中')
		})
	})
}).on('error', function(err) {
	console.log('错误信息：' + err)
})
```

2017-07-16 新增：
如果想处理抓取的html,可以使用cheerio模块，进行过滤，使用起来和jq类似，示例代码：
```js
let $ = cheerio.load(html)
let movieList = $('.grid_view li')
```

