---
title: "使用ohttps自带的docker-nginx来配置https服务"
date: 2022-01-08
draft: false
description: "使用ohttps自带的docker-nginx来配置https"
summary: "使用 ohttps 官方提供的nginx容器镜像ohttps/ohttps-nginx来配置网站https服务。"
tags: ["教程"]

cascade:
  showDate: true
  showAuthor: true
  showSummary: true
  invertPagination: true
---

## 介绍

[ohttps](https://ohttps.com)中`docker-nginx`类型的部署节点用于实现将 ohttps 中申请的证书部署至 nginx 容器中。

## 使用方法

使用`docker-nginx`类型的部署节点，使用 ohttps 官方提供的`nginx`容器镜像`ohttps/ohttps-nginx`。
该镜像是基于`nginx`官方稳定版镜像`nginx:1.16`构建，添加了证书更新服务后生成的。`ohttps/ohttps-nginx`镜像内的其他内容和使用方式和`nginx`官方镜像完全一致。

### 拉取镜像

```bash
 sudo docker pull ohttps/ohttps-nginx
```

### 添加`docker-nginx`部署节点

记录节点名称(`PUSH_NODE_ID`):`push-xxxxxxxxxx` <br />
记录令牌(`PUSH_NODE_ID`):`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### 配置`docker-compose.yml`文件

```yml
version: "2"

services:
  nginx:
    container_name: ohttps-nginx
    image: ohttps/ohttps-nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    environment:
      # 节点名称
      PUSH_NODE_ID: "push-xxxxxxxxxxxxx"
      # 令牌
      PUSH_NODE_TOKEN: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
    volumes:
      # nginx配置所在地
      - /etc/nginx/conf.d:/etc/nginx/conf.d
      # nginx日志所在地
      - /etc/nginx/logs:/var/log/nginx
      # 部署文件所在地
      - /opt:/opt
```

### 安装`docker-compose`并将其移动至/usr/local/bin

```bash
wget https://hub.fastgit.org/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64
mv docker-compose-linux-x86_64 docker-compose
chmod +x docker-compose
sudo mv docker-compose /usr/local/bin
```

### 运行`docker-compose.yml`文件并打开[ohttps 官网](https://ohttps.com)部署

```bash
sudo docker-compose -f docker-compose.yml up -d
```

启动之后，进入到对应的镜像，然后在`/etc/nginx/certificates`目录下看是否已经更新了对应的证书.

```bash
docker exec -it ohttps-nginx /bin/bash
cd /etc/nginx/certificates
ls
```

### 测试站点

## 附录

`http.conf`

```nginx
server {
    listen 0.0.0.0:80 default_server;
    listen [::]:80 default_server;
    server_name ~^(.*)$;
    rewrite ^(.*)  https://$host$request_uri;
}
```

`https.conf`

```nginx
server {
    listen 0.0.0.0:443 ssl http2;
    listen [::]:443 ssl http2;
    # cert-xxxx为证书目录
    ssl_certificate /etc/nginx/certificates/cert-xxxx/fullchain.cer;
    ssl_certificate_key /etc/nginx/certificates/cert-xxxx/cert.key;
    server_name badnl.com www.badnl.com;
    if ($host = badnl.com) {
        rewrite ^(.*)  https://www.badnl.com$request_uri;
    }
    root /opt/www.badnl.com/public;
    index index.html;
}

server {
    listen 0.0.0.0:443 ssl http2;
    listen [::]:443 ssl http2;
    # cert-xxxx为证书目录
    ssl_certificate /etc/nginx/certificates/cert-xxxx/fullchain.cer;
    ssl_certificate_key /etc/nginx/certificates/cert-xxxx/cert.key;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    add_header Strict-Transport-Security "max-age=31536000";
    client_max_body_size 200m;
    server_name dev.badnl.com ;
    location / {
        proxy_pass http://IP:PORT;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;
        proxy_set_header X-real-ip $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

server {
    listen 0.0.0.0:443 ssl http2;
    listen [::]:443 ssl http2;
    # cert-xxxx为证书目录
    ssl_certificate /etc/nginx/certificates/cert-xxxx/fullchain.cer;
    ssl_certificate_key /etc/nginx/certificates/cert-xxxx/cert.key;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
    ssl_prefer_server_ciphers on;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    add_header Strict-Transport-Security "max-age=31536000";
    server_name cdn.badnl.com;
    client_max_body_size 200m;
    root /opt/cdn.badnl.com;
    index index.html;
    location / {
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
        add_header 'Access-Control-Allow-Origin' '*';
        expires 30d;
        log_not_found off;
    }
}
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