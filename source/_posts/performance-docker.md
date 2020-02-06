---
title: 性能测试上docker
date: 2020-01-01 12:08:22
tags: [javascript, docker]
categories: [nodejs]
description: 没想到拖到现在才写
---

在服务器上部署，需要做什么呢？

### 项目配置
* 增加docker文件
 1. 增加pm2, 在package.json中的scripts中增加`"pm2": "pm2 startOrReload --no-daemon pm2.json"`
 2. 增加docker文件夹，内部创建Dockerfile、start.sh

```bash
# Dockerfile 文件
FROM xxx

WORKDIR xxx #设置工作路径

# 一系列copy动作
COPY 项目的路径 docker中的路径

# 安装git, 创建文件，设置文件权限
RUN apk add --no-cache git \
    && mkdir /apps/logs/ \
    && chown -R node:node /apps/logs/ \
    && yarn \
    && chmod +x /start.sh \
    && chown -R node:node /tmp

CMD ["/start.sh"]


# start.sh文件
#!/bin/sh

# 根据环境不同设置传入参数CONF_NAME
# 启动pm2
npm run pm2 --conf=/apps/conf/${CONF_NAME}
```

* 打镜像，测试机，推tag
 - docker build -t tagName -f docker/Dockerfile .
 - docker run -it -v 实际挂载路径:docker内部使用的路径 tagName

* 配置文件
 - 在不同环境使用不同的配置文件，配置文件的格式可以自己定义，然后在server启动的时候读取配置文件

### 开机启动
* docker-compose.yml

```bash
version: '3'
services:
  services_name:
    image: tagName
    container_name: container_name
    user: root # 用户启动身份
    restart: always # 跟随docker服务重启
    environment:  # 环境变量
      - HOST=${JMETER_PUBLISH_HOST}
    volumes: # 挂载变量
      - xxxx:/apps/conf/online.conf
      - xxxx:/home/works/saas-performance-material-hub
    ports: # 端口映射
      - 65534:65534
      - 8001:8001
```
* Redis 部署
 - 直接在上面的docker-compose文件中增加redis的配置

### 测试验证
项目运行之后，如何去查看log:`docker exec -it container-id bash/sh`


### 项目开发中的问题
1. 项目中需要操作的文件，加一个：如果不存在，先创建，保障项目的可用性
2. 项目中一定要有一个全局的try catch，一定要有app和process的

```js
app.on('error', (err) => {
  console.error(`app.error出现错误，${err.message}`)
})

process.on('uncaughtException', (err) => {
  console.error(`uncaughtException出现异常，${err.message}`)
})
```
3. 项目中的redis一定要增加重试机制

```js
options.retry_strategy = (options) => {
  // 前5次1分钟重连一次 超过五次，5分钟重连一次
  let retryInterval = 1000 * 60
  if (options.times_connected > 5) {
    retryInterval *= 5
  }

  console.log(`---redis 正在尝试重连, ${retryInterval / (1000 * 60)}分钟之后重连`)
  return retryInterval
}
```
4. js文件中的websocket的地址写成灵活的，不能写成死的
5. 一定要打出详细的log，以后好排查问题


### 遇到的问题
1. 项目中有获取ip的操作，在docker中获取不到正确的ip，可以选择使用环境变量的形式传进去
2. docker虽然类似linux，但base的docker不一样的话，可能存在命令不存在的情况。刚开始在项目中使用curl命令，在本机调试的时候正常，但是部署上去不可以了，进入docker查看后，发现不存在curl命令
3. 镜像导出导入的问题

```bash
docker save tag_id > filename.tar
wget -P /保存文件的目录 文件下载地址
docker load < filename.tar
docker tag tag_id xxxxx # 修改tag名字
```

4. docker开机自启动

```bash
systemctl enable docker.service

# 启动docker-compose中的服务
vim /etc/rc.d/rc.local
/usr/local/bin/docker-compose -f /www/docker/trace_fecshop/docker-compose.yml up -d
```

5. docker中的权限问题，这是一个很头疼的问题。即使相同的权限，在不同的机器有的能起来有的起不来。相同的docker在不同机器上启动情况不一样，可能是文件权限的问题。








