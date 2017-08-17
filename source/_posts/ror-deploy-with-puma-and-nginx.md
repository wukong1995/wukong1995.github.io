---
title: ror-deploy-with-puma-and-nginx
date: 2018-11-28 14:23:56
articleTitle: ror+psql+puma+nginx部署项目
tags: ['rails']
categories: ['rails']
description: ror+psql+puma+nginx部署项目
---
#### 前言
这个过程基本上我花了三个星期，才弄好，其中的有些部署文件还是我直接从项目复制粘贴过来的。现在一整理，发现并没有那么难，那为什么会花费三个星期呢？大部分可能是因为我的无知和对centos的不了解。

例如，我在启动数据库的时候，它告诉我不能以root身份启动，等我切换到普通账户，又告诉我，对文件夹没有写入权限，看得我真的很矛盾。

例如，我在安装pg这个gem的时候，告诉我缺乏依赖，我找到答案`yum install libs-devel`，又告诉我ruby的版本太低了，很纳闷，我明明装的是最新的ruby版本，之后就一直在缺乏依赖着转圈，等到现在才知道是因为找不到`pg_config`， `pg_config`这个文件一般是在`/usr/bin`文件夹下，但是我安装完没有在这....

#### 安装ruby
如果使用yum安装ruby，版本太低，为了管理ruby版本，我使用rbenv来管理
[安装参考链接](https://linuxize.com/post/how-to-install-ruby-on-centos-7/)

#### 安装postgresql
修改`/etc/yum.repos.d/CentOS-Base.repo`文件

```js
sudo vi /etc/yum.repos.d/CentOS-Base.repo
exclude=postgresql*    // 在[base]和[updates]块中加上这一行
```

安装postgresql
```js
sudo rpm -Uvh http://yum.postgresql.org/9.5/redhat/rhel-6-x86_64/pgdg-centos95-9.5-1.noarch.rpm
sudo yum install postgresql95 postgresql95-devel postgresql95-server postgresql95-libs postgresql95-contrib
```

初始化数据库
找到psql的安装目录(xxxx)的bin文件夹，

```js
xxxx/bin/pg_ctl -D xxxx/data initdb //初始化数据库
xxxx/bin/pg_ctl -D xxxx/data start  // 启动数据库
```

在initdb，可能会出现权限不够的情况，这个时候，查看一下`data`的权限，发现你应该将用户切换成`postgres`
```js
sudo su postgres
```
在这一步，会提示你输入密码，如果你不知道密码，就使用以下命令去修改密码

```js
passwd postgres
```

将用户切换成`postgres`就可以初始化和启动数据库，也可以使用以下命令

```js
sudo service postgresql-9.5 start               // 启动数据库
sudo chkconfig --levels 235 postgresql-9.5 on   // psql开机自启动
```

创建psql的链接用户名和密码
```js
sudo su postgres
psql
alter user username with password 'password';
create database testdb owner=username;
```


#### 为puma增加代理
```js
cap production puma:restar
```
这个时候访问端口，发现无法访问，查一下puma的log，发现`puma`在`production`环境下是使用`socket`链接，只能使用nginx做代理了，编辑`/etc/nginx/conf.d/default.conf`文件，替换下面代码中的`username`和`deploy_path`

```js
upstream app {
    # Path to Puma SOCK file, as defined previously
    server unix:///home/username/deploy_path/shared/sockets/puma.sock fail_timeout=0;
}

server {
    listen 80;
    server_name localhost;

    root /home/username/deploy_path/current/public;

    try_files $uri/index.html $uri @app;

    location @app {
        proxy_pass http://app;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
    }

    error_page 500 502 503 504 /500.html;
    client_max_body_size 4G;
    keepalive_timeout 10;
}
```
关于`puma`的`sock`的地址，你查一下`puma`的`log`确认一下

#### 你可能遇到的问题
1. 在安装pg这个gem的时候，可能会报缺少依赖，或者找不到pg_config文件
答：`gem install pg --with-pg-config=psql的安装目录/bin/pg_config`

2. 报错：ActiveRecord::StatementInvalid: PG::InsufficientPrivilege: 错误:  创建扩展 "uuid-ossp" 权限不够
答：需要为psql中的新建username设置super权限

3. 即使你添加了nginx代理，你发现，网站还是无法访问
答：可以查一下`/var/log/nginx/nginx_error.log`，我遇到的原因是权限不够，修改`nginx`的config，将`user`由`nginx`改成`root`


#### 后续
如果遇到瓶颈，不要再继续下去，放下来，明天再来尝试....
