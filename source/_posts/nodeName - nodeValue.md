---
  date: 2016-05-29 19:18:15
  title: nodeName - nodeValue
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---


html
<div id="container">
        <p>这是一个元素节点</p>
    </div>

js
<span style="font-size:18px;">var container = document.getElementById('container')
console.log(container.nodeName + "/" +container.nodeValue)
var attrNode = container.attributes[0]
console.log(attrNode.nodeName + "" +attrNode.nodeValue)
var textNode = container.childNodes[0]
console.log(textNode.nodeName + "" +textNode.nodeValue)
var commentNode = document.body.childNodes[1]
console.log(commentNode.nodeName + "" +commentNode.nodeValue)
console.log(document.doctype.nodeName + "" +document.doctype.nodeValue)
 var frag = document.createDocumentFragment()
console.log(frag.nodeName + "" +frag.nodeValue)</span>

