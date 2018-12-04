---
  title: CSS3实现立体菜单
  date: 2016-05-31 19:55:15
  tags: [css]
  categories:
    - [css]
    - [前端]
  description:
---


```html
<div class="nav">
    <a href="#" class="highlight">Home</a>
    <a href="#">About</a>
    <a href="#">Servics</a>
    <a href="#">Portfolio</a>
    <a href="#">Blog</a>
    <a href="#">Contact</a>
</div>
```

```css
* {
    margin:0;
    padding: 0;
    font-family: 'Microsoft Yahei'
}
a {
    color: #fff;
    text-decoration: none;
}
body {
    background-color: #222930;
}
.nav {
    margin:10%;
}
.nav a {
    padding: 15px 20px;
    margin-left: -4px;
    border: 1px solid #121212;
    border-right: none;
    color: #777;
    cursor: pointer;
    font-size: 14px;
    box-shadow: 0 2px 3px rgba(255,255,255,0.1) inset,
                0 1px 0px rgba(255,255,255,0.1) ;
}
.nav a:first-child {
    border-radius:6px 0 0 6px;
}.nav a:last-child {
    border-radius: 0 6px 6px 0;
    border-right: 1px solid #121212;
}
.nav a.highlight {
    color: #fff;
}





