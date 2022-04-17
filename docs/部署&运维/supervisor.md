---
title: supervisor
date: 2021-10-31 13:07:31
permalink: /pages/0973f0/
categories:
  - 部署&运维
tags:
  - 
---
## Config file
```
[program:video1]
command=python xyz_database_auto_updater.py --url=mysql://xx:xx@xx --logzip=tmp_storage1 --instance_type=video
directory=/home/xyz/data-uploader/scripts
user=xyz
autostart=true
autorestart=true
redirect_stderr=true
environment=PYTHONPATH="/home/xyz/.local/lib/python2.7/site-packages"
stdout_logfile=/mnt/log/video_log_tmp_storage1.txt
stderr_logfile=/mnt/log/video_log_tmp_storage1.txt
```

## 注意点
1. 报错No module named django
    ```
    环境变量的问题，把PYTHONPATH带进去
    ```
2. boto3报错：无法定位凭据
    ```
    用户组的问题，把root改为当前账户
    user=root   ----->   user=xyz
    ```