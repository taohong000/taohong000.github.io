---
title: linux服务器配置
date: 2017-08-16 15:08:41
categories: 
- 技术
tags: 
- linux
---

# linux服务器配置

## 更新:yum
```bash
yum -y update
```

## 下载软件
```bash
yum -y install wget
```

## 解压软件
```bash
yum -y install tar
```

## 编辑器
```bash
yum -y install vim
```

## 安装nginx
```bash
yum -y install nginx
```

## nodejs版本管理
```bash
git clone https://github.com/creationix/nvm.git ~/.nvm && cd ~/.nvm && git checkout `git describe --abbrev=0 --tags`
. ~/.nvm/nvm.sh
# 全局配置
vi ~/.bashrc
# 加一句
source ~/.nvm/nvm.sh
```
## 安装 Nodejs
nvm install 4.4.3

## 淘宝镜像
```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 安装pm2
```bash
cnpm install pm2 -g
```

## 写入配置文件
```bash
# 进入配置文件目录
cd /etc/nginx/conf.d/
# 新建配置
vi blog.conf
```

服务配置
``` bash
server {
   listen 80;
   server_name blog.taohong.space; #域名

   location / {
     proxy_pass http://127.0.0.1:3000; # 端口
   }
}
```

静态文件配置
``` bash
server {
   listen 80;
   server_name resume.taohong.space; #域名

   location / {
     root   /project/resume/docs; #静态文件地址
     index  index.html index.htm; #入口文件
   }
}
```

## 重启nginx
```bash
nginx -s reload
```