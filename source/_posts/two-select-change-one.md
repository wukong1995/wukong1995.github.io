---
title: two-select-change-one
date: 2018-10-22 19:30:25
articleTitle: 你能把这两个查询变成一个吗
tags: ['pg']
categories: ['数据库']
---

今天写了一个方法，被问到这两个查询能改成一个查询吗？
```ruby
def get_producer_name
  str = producers.limit(3).map{ |producer| producer.name }
  str += '等' if producers.count > 3
end
```

在上面的代码中，`producers.limit(3)`和`producer.count`会进行两次查询，要变成一个查询，肿么办？

```ruby
def get_producer_name
  pros = producers.limit(4)
  str = pros[0...2].map{ |producer| producer.name }
  str += '等' if pros.length > 3
end
```

一次性查出来前四条！这个方式真的很赞👍！是否给了你新的idea🤔️
