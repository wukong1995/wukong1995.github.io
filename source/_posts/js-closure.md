---
  title: js点击列表的第一个元素，不起作用，最后一个元素起作用(闭包)
  date: 2016-06-06 20:46:28
  tags: [javascript]
  categories: [前端, javascript]
  description:
---

在网上看到 手风琴菜单的demo，就想来模仿着做一个

```html
<div class="container">
    <ul>
        <li class="dropdown">
            <a href="#" data-toggle="dropdown">First Menu <i class="icon-arrow"></i></a>
            <ul class="dropdown-menu">
                <li><a href="#">Home</a></li>
                <li><a href="#">About Us</a></li>
                <li><a href="#">Services</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </li>
        <li class="dropdown">
            <a href="#" data-toggle="dropdown">Second Menu <i class="icon-arrow"></i></a>
            <ul class="dropdown-menu">
                <li><a href="#">Home</a></li>
                <li><a href="#">About Us</a></li>
                <li><a href="#">Services</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </li>
        <li class="dropdown">
            <a href="#" data-toggle="dropdown">Third Menu <i class="icon-arrow"></i></a>
            <ul class="dropdown-menu">
                <li><a href="#">Home</a></li>
                <li><a href="#">About Us</a></li>
                <li><a href="#">Services</a></li>
                <li><a href="#">Contact</a></li>
            </ul>
        </li>
    </ul>
</div>
```

```js
window.onload = function() {
    var dropdown = document.querySelectorAll('.dropdown');

    for (var i = 0; i < dropdown.length; i++) {
        var button = dropdown[i].querySelector('a[data-toggle="dropdown"]'),
            menu = dropdown[i].querySelector('.dropdown-menu'),
            icon = button.querySelector('.icon-arrow');

        addEvent(button, "click",function(event) {
            // var menu = this.nextElementSibling,
            //   icon = this.querySelector('.icon-arrow');

            if(hasClass(menu,"show")) {
                menu.classList.remove("show");
                icon.classList.remove("open") ;
            } else {
                menu.classList.add("show");
                icon.classList.add("open");
            }
            event.preventDefault();
        });
    }

    function hasClass(ele,className) {
        return ele.className && new RegExp("(^|\\s)" + className + "(\\s|$)").test(ele.className);
     }
}
```

发现错误，无论我点击那一个li列表，有效果的<ul class="dropdown-menu"> 总是最后一个，那麽出错原因应是 menu、 icon的获取，但是我还是没找到 原因，于是就修改了一下，改为在button事件中获取menu、button，这次对了，可是我还是不知道我的错在哪？求各位指点。。。。。

原因是：当为button绑定时间时，for循环早就走完了，于是改正一下，将for循环中的内容放在闭包中，改为一下写法：
```js
for (var i = 0; i < dropdown.length; i++) {
  (function() {
      var button = dropdown[i].querySelector('a[data-toggle="dropdown"]'),
          menu = dropdown[i].querySelector('.dropdown-menu'),
          icon = button.querySelector('.icon-arrow');

      addEvent(button, "click",function(event) {
          // var menu = this.nextElementSibling,
          //     icon = this.querySelector('.icon-arrow');

          if(hasClass(menu,"show")) {
              menu.classList.remove("show");
              icon.classList.remove("open") ;
          } else {
              menu.classList.add("show");
              icon.classList.add("open");
          }
          event.preventDefault();
      });
  })(i);
}

```css
* {
    margin: 0;
    padding: 0;
    font-family: "microsoft yahei";
    box-sizing: border-box;;
}
a {
    color: #000;
    text-decoration: none;
}
ul {
    list-style: none;
}
.container {
    width: 100%;
    height: auto;
}
.container> ul {
    width: 320px;
    height: auto;
    margin: 50px auto;
}
.container ul .dropdown {
    width: 100%;
    position: relative;
}
.container ul .dropdown a {
    display: block;
    width: 100%;
    height: 100%;
    background-color: #2980b9;
    line-height: 40px;
    padding-left: 10px;
    color: #fff;
    box-shadow: 0 -1px 0px #409ad5 inset,0 -1px 0px #20638f inset;
}
.container ul .dropdown a .icon-arrow {
    position: absolute;
    right: 10px;
    top: 17px;
    width: 12px;
    height: 12px;
    transition: all .3s;
}

.container ul .dropdown a .icon-arrow:after {
    content: "";
    border:6px solid transparent;
    border-top: 6px solid #fff;
}
.container ul .dropdown a .icon-arrow.open {
    transform: rotate(-180deg)
}
.container ul .dropdown .dropdown-menu {
    display: none
}
.container ul .dropdown .dropdown-menu.show{
    display: block
}
.container ul .dropdown .dropdown-menu a {
    background-color: #EEEEEE;
    color: #6e6e6e;
    box-shadow: 0 -1px 0px #C3C3C3 inset;
}
```


