---
title: 在react中使用typescript
date: 2019-04-09 11:06:36
tags: [react, typescript]
categories: [前端, react]
description: 将jsx变成tsx
---

### 安装
```js
yarn add typescript
yarn add tslint tslint-config-prettier -dev
```

### 增加配置文件
增加配置文件tsconfig.json tslint.json
在webpack中增加loader
```js
module.exports = {
  test: /\.(ts|tsx)?$/,
  use: 'ts-loader'
};
```

### 改造
1. 更改prototypes

```js
// 使用interface 代替 组件的proptypes
interface ItemProps {
  path: string,
  title: string,
  description: string,
  published_at: string,
  remote: boolean,
  isHasFooterLeft: boolean,
}

class ArticleItem extends React.Component<ItemProps> {
  private static defaultProps = {
  }
}
```
我之前习惯于将proptypes和defaultProps写在组件的外面，更改了proptypes，那么defaultProps为了代码的清晰度，就移入了组件的内部.

2. 更改state

```js
const initialState = { checkedTags: [] };
type State = Readonly<typeof initialState>;

class FilterWrappper extends React.Component<IProps> {
  readonly state: State = initialState;
  // xxx
}
```
props中的类型使用ts定义的话，类型名需要改变，我在设置类型functuon的时候，找了好久才找到正确的名字。

组件中的函数使用private或者public需要再斟酌一下。目前生命周期函数使用public，自定义的函数使用private。

### 遇到的问题
使用ts-loader，ts文件中不能解析webpacke中alias，需要将webpack中的alias复制到tsconcifg文件中，形如：
```js
"baseUrl": "./",
"paths": {
  "@components/*": ["frontend/components/*"],
}
```

### 是否有必要
引入ts最想要的是类型检查，但是引入ts的成本很高，如果对现有项目进行全部的迁移，工作量是巨大的。ts开始支持jsdoc了，以注释的形式增加类型支持。

是否使用ts，根据实际情况进行选择。
