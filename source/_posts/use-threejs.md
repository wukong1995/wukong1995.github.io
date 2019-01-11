---
title: threejs初试
date: 2019-01-11 22:02:01
tags: [javascript]
categories: [前端, javascript]
description: 被threejs闪瞎了眼
---

最近需要一个效果，去看了demo，发现是使用three.js做的。虽然是第二次看threejs的demo，但是被闪瞎了眼，真的是太酷了（我需要去补点图形学的知识）。

api太长了，我准备先去找点demo的代码试一下，额，这代码看不懂啊，这是些什么？虽然不太清楚，先写出来能看的demo试试。找了好几圈，我不知道我想要的效果是使用什么api来解决。还是都看一下吧。。。

第二天，我带着满肚子的疑问先看了文档，终于对前一天看到的api有了大致的认识。具体细节的参数，还是需要斟酌一下。

我想要的是漫天繁星的效果，仔细想想的话，首先有两个重要的点：画星星、怎么让页面动起来（这个效果应该是像拿着望远镜一样看天空的感觉）

1. 画星星
```js
for (let i = 0; i < 300; i++) {
    let geometry = new THREE.SphereBufferGeometry(.6, 2, 2);;
    let material = new THREE.MeshBasicMaterial({
      color: 0x4F4F4F
    });
    let sphere = new THREE.Mesh(geometry, material);
    sphere.position.set(
      Math.random() - 0.5,
      Math.random() - 0.5,
      -Math.random() * 0.5
    ).normalize().multiplyScalar(getRandomFloat(100, 300));
    scene.add(sphere);
    stars.push(sphere);
  }
```

2. 如何动起来
为页面增加鼠标移动监听事件，在事件函数中改变全局变量

```js
document.addEventListener('mousemove', onDocumentMouseMove, false);

function onDocumentMouseMove(event) {
  mousePosition.x = event.clientX;
  mousePosition.y = event.clientY;
  normalizedOrientation.set(
    -((mousePosition.x / screenWidth) - 0.5) * cameraAmpl.x,
    ((mousePosition.y / screenHeight) - 0.5) * cameraAmpl.y,
    0.5,
  );
}
```

[代码戳这里](https://medium.freecodecamp.org/javascript-objects-square-brackets-and-algorithms-e9a2916dc158)


