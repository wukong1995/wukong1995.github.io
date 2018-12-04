---
  title: javaScript中的事件代理
  date: 2016-05-28 10:38:42
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---


今天在看视频时，发现了 事件代理 这一方法。
假设在一个div中，有很多button，很多li标签，它们需要绑定相应的方法，如果一个一个写就太麻烦了，这时候事件代理的优点就凸显出来了。
下面看一下具体例子

```js
for (var i = 0; i < boxs.length; i++) {
	// 1）在li上绑定点击事件代理
	boxs[i].onclick = function(event) {
			// e = e || window.event;
			//1.获取触发元素，取得class。
			var el = event.target || event.srcElement;
			// this 指的是box ，el指的是当前点击的元素
			//2.根据class调用不同的函数。
			switch (el.className) {
				case 'close':
					removeNode(this);
					break;
				case 'praise':
					praiseBox(this, el);
					break;
				case 'btn':
					replayBox(this, el);
					break;
				case 'comment-praise':
					praiseReply(el.parentNode);
					break;
				case 'comment-operate':
					operateReply(this, el);
					break;
			}
		}
}
这样就简单很多了吧。。。



