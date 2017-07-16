---
title: install_ftp
date: 2017-07-09 22:25:02
tags: Linux
categories: 技术
---

# 安装ftp服务

## 安装

```bash
rpm -qa|grep vsftp
yum install vsftpd -y
```

## 启动服务

```bash
service vsftpd start
```

## 添加ftp用户

```bash
adduser test
passwd test
```