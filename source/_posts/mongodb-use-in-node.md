---
  date: 2016-12-11 13:10:59
  title: nodejs中使用mongoose保存数据
  tags: ['mongoDB', 'nodejs']
  categories: ['mongoDB', '数据库', 'nodejs']
  description:
---


最近在学习mongdb

以下是使用mongoose模块来保存数据

```js
var mongoose = require('mongoose');
var moment = require('moment');

// 连接字符串格式为mongodb://主机/数据库名
mongoose.connect('mongodb://localhost/test');
var db = mongoose.connection;
//输出连接日志
db.on('error', function callback() {
	console.log("Connection error");
});

db.once('open', function callback() {
	console.log("Mongo working!");
});

// 创建schema
var Schema = mongoose.Schema;
var userSchema = new Schema({
	name: String,
	age: Number,
	createTime: {
		type: Date,
		default: moment().format('YYYY-MM-DD HH:mm:ss')
	},
	updateTime: {
		type: Date,
		default: moment().format('YYYY-MM-DD HH:mm:ss')
	},
	telphone: String
});
// 构建model
var User = mongoose.model('User', userSchema);
//构建model实例
var userData = new User({
	name: 'root',
	age: 21,
	telphone: '18766560229'
});

// 保存数据
userData.save(function(err) {
	if (err) {
		console.log(err)
	} else {
		console.log('Save success');
	}
})
```



