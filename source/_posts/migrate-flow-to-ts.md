---
title: migrate-flow-to-ts
date: 2019-06-01 16:35:51
tags: [typescript]
categories: [前端, typescript]
description: ts一统天下
---

现在经常开发的两个项目，一个是用的ts，一个用的flow。ts是现在一个主流的技术，新同学进来需要学习flow的语法，在开发过程中，在flow项目中也经常会写ts的语法，但是在js文件里面写了ts也不会什么作用啊。

找了从flow迁移到ts的文章，跟着里面的步骤实践了一遍，过程繁琐了点，但结果还是令人欣喜的。
大致的步骤是：
* 更改文件后缀名
* 移除所有flow的注释
* 移除flow的配置文件和package，增加ts配置文件
* 在webpack中增加ts-loader
* 修复type error

#### 1. 更改文件后缀名
项目中所有的文件都是js文件，根据文件名可以选择后缀是tsx还是ts。使用fs为所有文件重命名。（这就体现了文件命名约定的好处了）

```js
const fs = require('fs');
const tsFileReg = /(style|styles|data|util|type|reducer|action|gql|index).js$/

const dfs = (path) => {
  fs.readdir(path, function (error, files) {
    if (error) {
      console.log(error);
    } else {
      files.forEach((filename) => {
        const currentPath = path + filename;
        const stats = fs.statSync(currentPath);
        if (stats.isDirectory()) {
          // 文件夹
          dfs(currentPath + '/');
        } else {
          // 文件重命名
          let newSuffix = tsFileReg.test(filename) ? '.ts' : '.tsx'
          const newFileName = filename.replace(/\.js$/, newSuffix);

          fs.rename(currentPath, `${path}${newFileName}`, function (err) {
            if (err) throw err;
            console.log('renamed complete');
          });
        }
      });
    }
  });
}

dfs('./src/shared/');
```

虽然在重命名的时候增加了规则，但是还是会出现错误的后缀名，原因是有一些文件命名不符合现在的命名规则。

#### 2.移除所有flow的注释
文件太多，我做不到一个一个删除了，于是选择了使用编辑器将`// @flow`变成空

#### 3. 移除flow的配置文件和package，增加ts配置文件
1. 删除tools/scripts/flow.js和tools/scripts/removeTypes.js
2. 删除tools/flow/文件夹
3. 删除.flowconfig
4. 增加tsconfig.json
5. 增加依赖： @types/react、@types/react-dom、tslint、tslint-immutable、tslint-react、typescript、ts-loader
6. 删除依赖：eslint-plugin-flowtype、flow-typed

#### 4. Add tslint and autofix code style
增加tslint.json

#### 5. Remove type identifier in import interfaces
删除（import type { AppProps } from '../interfaces/app'）里面的type标示

#### 6. Adjust webpack config
```js
module.exports = {
  test: /\.tsx?$/,
  use: 'ts-loader'
};
```
文件扩展名增加：`ts, tsx`

#### 7. 修复type error和warning
使用`tslint -c tslint.json -p tsconfig.json -e \"**/*.d.ts\" \"src/**/*.{ts,tsx}\"`去查找type error修复，这个虽然很简单，但是一个大工程...

PS：2019/7/4号得知tslint被弃用了，现在用的是`typescript-eslint`



