---
title: 使用react的context实现一个简单i18n
date: 2020-07-25 13:50:42
tags: [前端]
categories: [前端, react]
description: 国际化
---

受withApollo的启示，仔细研究了一下context的API，在开发中还没用过context，但是想antd中的form这种跨组件传递数据，那i18n正适合啊

### 基本代码
先来三个基本组件，到时候准备替换的就是title和desc

```js
import React, { useState } from 'react'

const Title = () => {
  return (
    <div>title</div>
  )
}

const Descrption = () => {
  return (
    <div>desc</div>
  )
}

const Container = () => {
  return (
    <div>
      <Title />
      <Descrption />
    </div>
  )
}

export default Container
```

### 语言配置
准备了en和cn的两个版本的语言配置文件：

```js
// en.js
export default {
  locale: 'en',
  messages: {
    title: 'title',
    desc: 'this is description'
  }
}

// zh-CN.js
export default {
  locale: 'zh-CN',
  messages: {
    title: '中文',
    desc: '这是一段描述'
  }
}
```

加载语言配置，
```js
// load_language.js
import en from './en'
import zhCN from './zh-CN'

const list = {
  [en.locale]: en.messages,
  [zhCN.locale]: zhCN.messages
}
```
写到这，我想起来webpack的ignore的plugin，加载多语言配置，肯定会将所有的语言全都load，假如你只需要cn，那么其他的语言包对你来说都是没有用的。经常见的就是moment这个，你会使用ignorePlugin忽略它，然后再手动引入


### 使用context注入
1、 准备conext

```js
// context.js
import React from 'react'

const LanguageContext = React.createContext('zh-CN')

export default LanguageContext

```
2、向load_language增加消费：

```js
const t = (title) => {
  return (
    <LanguageContext.Consumer>
      {language => (
        list[language][title]
      )}
    </LanguageContext.Consumer>
  )
}
```
3、然后我就可以直接改造我的title和desc了：

```js
const Title = () => {
  return (
    <div>{t('title')}</div>
  )
}

const Descrption = () => {
  return (
    <div>{t('desc')}</div>
  )
}
```
4、Container中增加切换语言的按钮：

```js
  const [language, setLanguage] = useState('zh-CN')
  const changeLanguage = () => {
    setLanguage(language === 'zh-CN' ? 'en' : 'zh-CN')
  }

  return (
    <LanguageContext.Provider value={language}>
      <button onClick={changeLanguage}>切换语言：当前是{language}</button>
      <Title />
      <Descrption />
      <Descrption22 />
    </LanguageContext.Provider>
  )
}
```

ok，可以了

### 使用hoc注入
另外，t还可以使用hoc的形式注入
```js
export const withLanguage = (Component) => (props) => {
  return (
    <LanguageContext.Consumer>
      {
        language => (
          <Component {...props} t={(title) => list[language][title] } />
        )
      }
    </LanguageContext.Consumer>
  )
}
```
在组件中使用：
```js
const Descrption2 = ({ t }) => {
  return (
    <div>hoc: {t('desc')}</div>
  )
}

const Descrption22 = withLanguage(Descrption2)
```

走到这，一个简单的i18n就完成了。

PS: 如果中文网站要做国际化的话，可能最快的方式还是开发一套英文网站，再它的基础上做国际化。中文转英文，长度是由短变长，排版上改的会让人崩溃。





