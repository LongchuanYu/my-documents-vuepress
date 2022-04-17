---
title: MySql QA
date: 2021-10-31 13:37:18
permalink: /pages/aa9d99/
categories:
  - MySql&ORM
tags:
  - 
---
## 把字段类型从BIGINT改为TEXT时出现错误
[ 问题描述 ]
```
1170错误 BLOB/TEXT Column Used in Key Specification Without a Key Length
```
[ 解决方案 ]
```
改成varchar(256)
```

## 聚集索引和非聚集索引？
1. 聚集索引
```
类比：字典按照拼音顺序查询，数据的物理地址和id顺序相同，id较大的，物理地址也相对靠后
```
2. 非聚集索引
```
类比：字典按照部首查询，查询的数据和物理地址并不按顺序
```

## MySQL8.0设置root密码
1. 用系统root用户登录mysql
```
sudo mysql -uroot
```
2. 修改密码
```
# localhost表示只能在本地登录，如果要远程登录，把localhost换成%
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```
3. 重启mysql service
```
sudo systemctl restart mysql.service
```

## mysql远程连接不上的解决方案
1. 云服务器防火墙设置，把3306设置为允许
2. 将mysql配置文件里面： bind_address一行注释掉