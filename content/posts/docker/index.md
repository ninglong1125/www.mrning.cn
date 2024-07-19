---
title: "docker命令简单记录"
date: 2024-06-25
lastmod: 2024-07-19
draft: false
description: "docker命令简单记录"
summary: "docker死忠粉了，记录一下每次部署服务时运行的重复代码。"
tags: ["教程","docker"]
editLink: www.mrning.cn
cascade:
  showDate: true
  showAuthor: true
  showSummary: true
  invertPagination: true
  showEdit: true
  showDateUpdated: true
---

## 介绍

在进行web开发时，我们经常需要部署多个服务，比如MySQL，Redis，MongoDB等，而这些服务如果去官网进行编译安装，则会非常麻烦，因此我们需要使用docker容器来部署这些服务。

本文中，我们将记录一下每次部署服务时都会运行的重复代码。
## 使用方法

本文假定读者已经了解`docker`的基本用法，并熟悉docker的相关命令及使用方法。
在部署服务时，我们通常遇到以下问题：端口映射、挂载目录、用户权限等等。
[docker-library](https://github.com/docker-library/docs)里有大量的docker运行命令，即便你不熟悉某个容器，也可以通过搜索找到相应的命令。

本文将把常用的服务部署命令记录下来，以便日后查阅。
## docker部署MySQL
```bash
sudo docker run -d \
    --name mysql \
    --restart unless-stopped \
    -p 3306:3306 \
    -v /path/to/mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=root \
    mysql:8.0.31
```
## docker部署Redis
```bash
sudo docker run -d \
    --name redis \
    --restart unless-stopped \
    -p 6379:6379 \
    -v /path/to/redis/data:/data \
    redis redis-server

```
## docker部署MongoDB
```bash
sudo docker run -d \
    --name mongo \
    --restart unless-stopped \
    -p 27017:27017 \
    -v /path/to/mongodb/data:/data/db \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=admin \
    mongo:4.4.9

```
## docker部署RabbitMQ
```bash

sudo docker run -d \
    --hostname my-rabbit \
    --name some-rabbit \
    --restart unless-stopped \
    -p 5672:5672 \
    -p 15672:15672 \
    -v /path/to/rabbitmq/data:/var/lib/rabbitmq \
    -e RABBITMQ_DEFAULT_USER=user \
    -e RABBITMQ_DEFAULT_PASS=user \
    rabbitmq:3.13-management
```

## docker部署MinIO 
```bash

sudo docker run -d \
    --console-address ":9001"
    --name minio \
    --restart unless-stopped \
    -p 9000:9000 \
    -p 9001:9001 \
    -v /path/to/minio/data:/data \
    -e "MINIO_ROOT_USER=admin" \
    -e "MINIO_ROOT_PASSWORD=admin" \
    quay.io/minio/minio server /data 
```

## docker部署qinglong
```bash
sudo docker run -dit \
    --hostname qinglong \
    --name qinglong \
    --restart unless-stopped \
    -p 5700:5700 \
    -v path/to/ql/data:/ql/data \
    -e QlBaseUrl="/" \
    -e QlPort="5700" \
    whyour/qinglong
```
## docker部署code-server
```bash
sudo docker run -d \
    --name code-server \
    -p 18080:8080 \
    -p 1313:1313 \ 
    -v /mnt/-1/docker-run/codeserver/.config:/opt/coder/.config \
    -v /mnt/-1/docker-run/codeserver/projects:/opt/coder/project \
    -e "DOCKER_USER=coder" \
    -u "coder" \
    codercom/code-server
```

## docker部署AList
```bash
sudo docker run -d \
    --restart unless-stopped \
    -v /mnt/-1/docker-run/alist:/opt/alist/data  \
    -p 5244:5244 \
    --name "alist" xhofe/alist:latest
```

-------------
<div>
如果这里的内容对您有那么一点儿帮助，您可以通过以下方式投喂支持。
{{<figure
    src="https://pic.imgdb.cn/item/62cbcb4ef54cd3f9379479c1.jpg"
    alt="投喂列表"
    caption="[投喂列表](/reward)"
    >}}</div>

------------