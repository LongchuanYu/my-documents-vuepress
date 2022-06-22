## Loki配置
#### 配置文档
路径： `~/LPG/loki/local-config.yml`
```yml
auth_enabled: false

server:
  http_listen_port: 3100	# 这个不需要变化, 即使loki接收端口变, 他也不用改

ingester:
  lifecycler:
    address: 0.0.0.0  # loki运行在docker里面，要想访问loki，需要把地址这样设置
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1
    final_sleep: 0s
  chunk_idle_period: 5m
  chunk_retain_period: 30s
  max_transfer_retries: 0
  wal:
      enabled: false

schema_config:
  configs:
    - from: 2021-06-04
      store: boltdb
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 168h

storage_config:
  boltdb:
    directory: /tmp/loki/index

  filesystem:
    directory: /tmp/loki/chunks

limits_config:
  enforce_metric_name: false
  reject_old_samples: true
  reject_old_samples_max_age: 168h
  ingestion_rate_mb: 15

chunk_store_config:
  max_look_back_period: 0s

table_manager:
  retention_deletes_enabled: false
  retention_period: 0s
```

## promtail配置
#### 配置文件
路径： `~/LPG/loki/local-config.yml`
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://172.16.0.71:3100/loki/api/v1/push  # 重要，设置为loki主机的IP地址

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log

```

## docker-compose配置
路径： `~/LPG/docker-compose.yml`
```
version: "3"

services:
  loki:
    image: grafana/loki
    container_name: lpg-loki
    volumes:
      - /home/xyz/LPG/loki:/etc/loki
    ports:
      - 3100:3100
    command: -config.file=/etc/loki/local-config.yaml

  promtail:
    image: grafana/promtail
    container_name: lpg-promtail
    volumes:
      - /mnt/log/data_uploader_log:/var/log/
      - /home/xyz/LPG/promtail:/etc/promtail/
    command: -config.file=/etc/promtail/promtail.yml

  grafana:
    image: grafana/grafana
    container_name: lpg-grafana
    ports:
      - 3000:3000

```

## 开启所有服务
在路径`～/LPG`下执行：
```
docker-compose up -d
```

## Troubleshooting
#### loki启动报错：wal ... permission denied ...
解决方案：
```
ingester:                      
  略.                                                                    
  wal:                                                                                         
      enabled: false    
```

#### grafana添加Datasource的时候报错：
报错信息：
```
Unable to fetch labels from Loki (Failed to call resource), please check the server logs for more detail
```
查看promtail发现报错：
```
msg="Failed to call resource" error="Get \"http://localhost:3100/loki/api/v1/labels?start=1655883782464000000&end=1655884382464000000\": dial tcp 127.0.0.1:3100: connect: connection refused"
```
解决方案：
```
把loki和promtail配置文件里面的ip地址改为主机ip地址
127.0.0.1 => 192.168.31.86
```


## 参考
1. https://www.codeleading.com/article/13505849587/