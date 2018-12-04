---
  title: document的createDocumentFragment()方法
  date: 2016-05-29 18:55:09
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---


假如你想动态的向html中添加十个段落，使用常规的方式可能会写出这样的代码：
```js
for(var i = 0 ; i < 10; i ++) {
    var p = document.createElement("p");
    var oTxt = document.createTextNode("段落" + i);
    p.appendChild(oTxt);
    document.body.appendChild(p);
}
```

当然，这段代码运行是没有问题，但是它调用了十次`document.body.appendChild()`，每次都要产生一次页面渲染。这时碎片就十分有用了：
```js
var oFragment = document.createDocumentFragment();
for(var i = 0 ; i < 10; i ++) {
  var p = document.createElement("p");
  var oTxt = document.createTextNode("段落" + i);
  p.appendChild(oTxt);
  oFragment.appendChild(p);
}
document.body.appendChild(oFragment);
```

在这段代码中，每个新的<p/>元素都被添加到文档碎片中，然后这个碎片被作为参数传递给`appendChild()`。这里对`appendChild()`的调用实际上并不是把文档碎片本省追加到body元素中，而是仅仅追加碎片中的子节点，然后可以看到明显的性能提升，`document.body.appenChild()`一次替代十次，这意味着只需要进行一个内容渲染刷新

