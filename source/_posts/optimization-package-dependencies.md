---
title: 优化项目中的包依赖
date: 2019-08-04 18:27:37
tags: [前端]
categories: [前端]
description: 如果可以的话，不要一个package存在多个版本
---

跑了一下webpack analyse，发现项目中有两个moment版本，查了一下antd中依赖的moment版本和项目依赖的版本不同。

如何统一版本呢？

第一个计划，开始时想着不在项目中引用moment，直接使用antd中的moment，但是想到新来的同学会不会有疑问，项目里面明明没有引入moment但是为什么能用？于是，这个想法被弃用了。

第二个计划，我本来想着直接将项目中的moment版本直接设置成2.x，但是和同事商量了一下，依赖项目的依赖的依赖这个事情不太靠谱，看了antd4的计划又可能会对moment下手，因为moment太大了，有人在issue上提了可不可以将moment换成dayjs。antd的计划是在秋天，哦，秋天已经不远了。

于是采用的方法是写一个脚本来判断项目中依赖和依赖的依赖的moment版本是否一致

```js
const { readFileSync } = require('fs')
const antdPath = 'node_modules/antd/package.json'
const projectPath = 'package.json'

try {
  const momentVersionInAntd = getMomentVersion(readFileSync(antdPath));
  const momentVersionInProject = getMomentVersion(readFileSync(projectPath));
  if (momentVersionInAntd === momentVersionInProject) {
    console.log(`moment的版本一致, 版本号是${momentVersionInAntd}`)
  } else {
    console.log(`版本不一致，antd的版本是${momentVersionInAntd}，项目中的版本是${momentVersionInProject}，请查看changelog以统一版本`)
  }

} catch (err) {
  console.log('---出现错误,原因:', err)
}

function getMomentVersion(content) {
  return JSON.parse(content).dependencies.moment
}
```

统一了版本后，打包后的文件就少出来一个moment...

在就是lodash的tree shake，选的方法是使用lodash-es来代替lodash，可以进一步缩小js文件的大小。

```js
import { get } from 'lodash';
// 变成下面这样的引用
import get from 'lodash-es/get';
```

我看现在好多实践中都是酱node_modules中的全部达成一个包，现在项目里面也是这样的，但是光node_modules里面的打出来压缩完就3.9M这个也太多了，于是我想直将引入两次及以上的打进去，其他的单独是一个chunk，这样的话能从3.9变成2，只是试了一下，但还没有完整的探索一下～下周再试试






