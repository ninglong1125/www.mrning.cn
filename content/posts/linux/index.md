---
title: "Linux命令简单记录"
date: 2022-09-09
draft: false
description: "Linux命令简单记录"
summary: "记录几个我经常使用，但是却经常忘的Linux命令。"
# slug: "getting-started"
tags: ["教程"]

cascade:
  showDate: true
  showAuthor: true
  showSummary: true
  invertPagination: true
---


## Linux命令简单记录 



 #### 创建用户并分配bash
```zsh
useradd <user> -s /bin/bash
```

 #### 更改用户密码
```bash
passwd <username>
```


 #### 更改文件(夹)用户权限
```bash
chown -R <username> <filename>
```


 #### 查询进程
```bash
ps -aux | grep <name>
```


 #### 查询端口
```bash
lsof -i :8080
```


 #### 查询内存
```bash
free -h
```


 #### 查询磁盘
```bash
df -h
```


 #### 查询当前文件夹占用空间
```bash
du -sh
```


 #### 统计文件夹下的文件夹数目
```bash
ls | wc -w
```


 #### 查看文件后几行内容
```bash
 tail -n 5 <filename>
```


 #### 删除某一目录下的所有特定类型文件
```bash
find <dirname> -name "*.ckpt" | xargs rm -rf
```


 #### 解压*.tar.gz文件

```bash
tar -xvzf *.tar.gz
```


 #### 解压*.tar.xz文件
```bash
tar -xvf *.tar.xz
```


 #### 将文件夹压缩成*.tar.gz
```bash
tar -zcvf <dirname>.tar.gz <dirname>
```


 #### 将文件夹压缩成*.zip
```bash
zip -q -r <dirname>.zip <dirname>
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