---
title: gunicorn
date: 2021-10-31 13:07:31
permalink: /pages/1700db/
categories:
  - 部署&运维
tags:
  - 
---
## 基础使用  
```
• 启动：
	gunicorn -w 3 madblog:app -b 0.0.0.0:5000

• 配置文件：gunicorn.conf.py

	import multiprocessing
	bind = '0.0.0.0:5000'
	workers = multiprocessing.cpu_count() * 2 + 1
	# daemon = True
	pidfile = '/run/gunicorn.pid'
	loglevel = 'info'
	errorlog = '/tmp/gunicorn-error.log'
	accesslog = '/tmp/gunicorn-access.log'
	access_log_format = '%(h)s %(l)s %(u)s %(t)s "%(r)s" %(s)s %(b)s "%(f)s" "%(a)s"'
```  