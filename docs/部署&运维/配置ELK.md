---
title: 配置ELK
date: 2022-04-17 16:30:18
permalink: /pages/f467e8/
categories:
  - 部署&运维
tags:
  - 
---
## 概览

## 1. filebeat
#### 配置文档
```yml
# filebeat.yml

filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /home/liyang/tmp/log.log

processors:
  # 用于分割message
  - dissect:
      tokenizer: "%{datetime}|%{log_level}|%{uuid}|%{server_name}|%{file_name}|%{msg}"
      field: "message"
      target_prefix: ""

  # 丢弃冗余的字段
  - drop_fields:
      fields: ["log","host","input","agent","ecs"]
      ignore_missing: false
```
#### 启动
```
./filebeat -e -c filebeat.yml -d "publish"
```

## 2. logstash
#### 配置文档
```
# 配置文档路径：logstash/config/logstash-sample.comf
# log格式：2022-01-08 13:52:41|INFO|26b41d81|LiYang_test|1640749070586_207000117475_0_rgb.png|process success


input {
  beats {
    port => 5044
  }
}
filter {
  mutate {
    split => ["message","|"]
    add_field => { "timestamp" => "%{[message][0]}" }
    add_field => { "error_type" => "%{[message][1]}" }
    add_field => { "prefix" => "%{[message][2]}" }
    add_field => { "server_name" => "%{[message][3]}" }
    add_field => { "file_name" => "%{[message][4]}" }
    add_field => { "info" => "%{[message][5]}" }
  }
}
output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    action => "index"
    index => "filebeat-%{+YYYY.MM.dd}"
  }
  # stdout { codec => rubydebug }
}

```

#### 启动
```
./bin/logstash -f config/logstash-sample.conf --config.reload.automatic
```

## 3. elasticsearch

#### 启动
```
./bin/elasticsearch
```

## 问题排查
#### 部署到服务器后通过ip地址访问不到
```
我用的腾讯云，把9200,9300添加到防火墙规则列表中，并设置为允许
```

#### filebeat直接输出到ecs，创建的index字段太多，大约有2万行
```
是ecs的index template导致的，在Kibana -> stack management -> index management -> index templates下，删除匹配到的template即可
```