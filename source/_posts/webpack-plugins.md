---
title: webpack官网所有的plugin
date: 2019-11-24 16:32:04
tags: [javascript]
categories: [前端, webpack]
description: This makes webpack flexible.
---
#### dllPlugin
这个plugin，webpack说是提高build time，它会将noede_modules里面的文件先build出来一个文件，每次hot reload的时候，只需要重新编译业务代码即可。

当然也可以使用它来提取多个项目的公共包，prodution也可以用[参考链接](https://github.com/webpack/webpack/issues/4561)，相比external来说，唯一的缺点就是会比原来的包的体积稍大一些。

对于react这样的包，dev和prod使用的js是不一样的，你需要针对不同的环境打不同的包。

另外还以一个缺点就是，不能正确的找到babel target版本的文件。例如，你的target是es next，他不会找xxx.es.min.js，只会找xxx.min.js。这样的话，也会导致最后的包增大。

所以，可不可以在production环境使用dllpligin打包出来的js文件，看业务需求即可。

#### 下面是所有plugin
![image](https://res.cloudinary.com/dwudaridr/image/upload/v1574586276/blog/webpack-plugins-1.png)

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1574586276/blog/webpack-plugins-2.png)

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1574586276/blog/webpack-plugins-3.png)

![image](https://res.cloudinary.com/dwudaridr/image/upload/v1574586276/blog/webpack-plugins-4.png)

