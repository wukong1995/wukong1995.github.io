---
  date: 2016-04-23 12:22:59
  title: 利用nodejs搭建server端
  tags: ['nodejs']
  categories: ['nodejs']
  description:
---


新建一个js文件，保存为server.js，输入以下代码：
```js
// 新建server服务器
var http = require('http');

var hostname = '127.0.0.1';
var port = 3000;

var server = http.createServer(function(req, res) {
	// res.writeHead(200, {'Content-Type': 'text/html'});
	// res.writeHead(200, {'Content-Type': 'text/plain'});
	res.statusCode = 200;
	res.setHeader('Content-Type', 'text/html');
	// res.getHeader('content-type')

	res.write('<head><meta charset="utf-8"/></head>');
	// res.charset = 'utf-8';   不行

	var htmlDiv = '<div style="width: 200px;height: 200px;background-color: #f0f;">div</div>';
	res.write('<b>亲爱的，你慢慢飞，小心前面带刺的玫瑰...</b>');
	res.write(htmlDiv);

	// 有参数=先调用 res.write(data, encoding) 之后再调用 res.end().
	res.end('<h1>Hello world!</h1>');
});

server.listen(port, hostname, function() {
	// hostname是const类型时，可以用以下写法
	//console.log('Server running at http://${hostname}:${port}/');

	console.log('Server running at http://%s:%s', hostname, port);
	// console.log('Server running at http://' + hostname + ':' + port + '/');
});
```

打开控制台，进入server.js所在的文件夹 ，在命令行中输入 `node server.js `或者使用sublime设置好node的路径，按ctrl+B即可

在浏览器的地址栏中输入`http://127.0.0.1:3000/`

就可以看到网页的内容了
如果修改了server.js文件，浏览器网页中的内容不会改变，这时候就需要重启服务

打开浏览器刷新，就可以看到内容已经改变



