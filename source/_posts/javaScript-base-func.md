---
  title: javaScript-base-func
  date: 2016-04-23 19:43:01
  articleTitle: javaScript基本用法
  tags: ['javascript']
  categories: ['javascript', '前端']
  description:
---

1、document.write() 可用于直接向 HTML 输出流写内容。简单的说就是直接在网页中输出内容。
2、警告alert弹出框：alert（字符串或变量）
3、确认（confirm 消息对话框）：confirm(str); 返回boolean值
4、提问（prompt 消息对话框）：prompt(str1, str2);
    str1: 要显示在消息对话框中的文本，不可修改
    str2：文本框中的内容，可以修改
5、innerHTML 属性：Object.innerHTML 获得或修改元素内容
6、改变 HTML 样式：Object.style.property=new style;
7、控制类名（className 属性）：object.className = classname
8、定义数组： var myarr=new Array();
9、数组赋值： myarr[1]=" 张三";
10、事件：鼠标单击事件( onclick ）、鼠标经过事件（onmouseover）、鼠标移开事件（onmouseout）、光标聚焦事件（onfocus）、失焦事件（onblur）、内容选中事件（onselect）、文本框内容改变事件（onchange）、加载事件（onload）、卸载事件（onunload）
11、日期对象：var Udate=new Date(); var d = new Date(2012, 10, 1);var d = new Date('Oct 1, 2012');
12、日期对象常用方法：get/setDate()、get/setFullYear()、
get/setYear()、get/setMonth()、get/setHours()、get/setMinutes()、get/setSeconds()、get/setTime()、getDay()(星期)
13、返回指定位置的字符：stringObject.charAt(index)
14、返回指定的字符串首次出现的位置：stringObject.indexOf(substring, startpos)
15、字符串分割：stringObject.split(separator,limit)
16、提取字符串：stringObject.substring(starPos,stopPos) 
17、提取指定数目的字符：stringObject.substr(startPos,length)
18、Math对象常用方法：向上取整：ceil()、向下取整floor()、四舍五入round()、随机数random()
19、数组连接：arrayObject.concat(array1,array2,...,arrayN)
20、指定分隔符连接数组元素：arrayObject.join(分隔符)
21、颠倒数组元素顺序：arrayObject.reverse()
22、选定元素：arrayObject.slice(start,end)
23、数组排序：arrayObject.sort(方法函数)
24、JavaScript 计时器：setTimeout() clearTimeout() setInterval()  clearInterval()
25、History对象：window.history.[属性|方法]，length|back() forword() go()
26、Location对象：location用于获取或设置窗体的URL，并且可以用于解析URL。  location.[属性|方法] 
27、Navigator对象：Navigator 对象包含有关浏览器的信息，通常用于检测浏览器与操作系统的版本。
28、DOM中的方法：getElementsByName(name)、getElementsByTagName(Tagname)、elementNode.set/getAttribute(name)、elementNode.childNodes、node.firstChild、node.lastChild、elementNode.parentNode、nodeObject.nextSibling、nodeObject.previousSibling、appendChild(newnode)、insertBefore(newnode,node)、removeChild()、node.replaceChild
 (newnode,oldnew )、document.createElement(tagName)、document.createTextNode(data)、
29、窗口： document.documentElement.clientHeight/Width、 document.body.clientHeight/Width、document.body.scrollHeight/Width、document.documentElement.scrollHeight/Width、offsetHeight和offsetWidth、scrollLeft/Top、offsetLeft/Top

