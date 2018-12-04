---
  title: 将json格式的字符串转化成object对象
  date: 2016-05-06 13:45:23
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---

将一堆json数据的字符串，转换成js能认识的数据
例如：`var response = "{"state":1,"msg":"yes","count":1}";`
对其进行操作：`response = eval('(' + response + ')');` 或者`$.parseJSON(response)`
这样在js中就可以访问`response.state,response.msg,response.count`


