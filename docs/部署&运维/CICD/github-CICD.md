---
title: github-CICD
date: 2021-10-31 13:17:55
permalink: /pages/1eced3/
categories:
  - 部署&运维
tags:
  - 
---
## github CICD初探
这次实验主要包含：
1. push之后触发CD行为
2. 拉取ubuntu-latest
3. 在虚拟环境中创建目录dist
4. 通过ssh-deploy@v2.1.5把虚拟环境中的dist目录复制到服务器中

#### 流程

1. 创建ssh密钥对（generator SSH key pair）
	```
	ssh-keygen -m PEM -t rsa -b 4096 -C "remly@qq.com"

	在任意电脑中均可生成，目的是要得到密钥对（包含公钥和私钥）
	```

2. 使用生成的ssh密钥对  
	```
	1. 处理私钥： 把私钥填进github项目仓库的secrets中，详情见下一步

	2. 处理公钥： 公钥写进要部署的服务器`～/.ssh/authorized_keys` 文件中

	注： 公钥是一把锁，把它放到要部署的服务器中。 私钥则是钥匙，持有钥匙的人才能有权限进入
	```

3. 在github项目仓库中配置secrets
	```
	仓库 -> Settings -> Secrets -> New repository secret
	目的是为了把密钥隐藏起来，用变量来表示

	REMOTE_HOST:    sports.remly.xyz
	REMOTE_TARGET:  /home/ubuntu/mysports/frontend
	REMOTE_USER:    ubuntu
	SERVER_SSH_KEY: ssh生成的私钥
	```

4. 创建yml文档 
	```
	路径： [Your work dir]/.github/workflows/deploy.yml

	name: Node CI

	on: [push]

	jobs:
	build:
		runs-on: ubuntu-latest
		steps:
			- name: New folder 
			  run: mkdir dist
			- name: Deploy to Server
			  uses: easingthemes/ssh-deploy@v2.1.5
			  env:
				SSH_PRIVATE_KEY: ${{ secrets.SERVER_SSH_KEY }}
				ARGS: "-rltgoDzvO --delete"
				SOURCE: "dist/"
				REMOTE_HOST: ${{ secrets.REMOTE_HOST }}
				REMOTE_USER: ${{ secrets.REMOTE_USER }}
				TARGET: ${{ secrets.REMOTE_TARGET }}
	```

#### 注意
1. 注意--delete参数会删除 TARGET 路径中 SOURCE 没有的文件（达到增量操作的目的）。，要谨慎使用！！
2. ssh-deploy 是把 SOURCE 下的文件传送到 TARGET 目录下， 而不是吧 SOURCE 文件夹传过去！！
3. 如果目标服务器下没有 REMOTE_TARGET 文件夹，则会创建。 因此不用担心文件不存在的问题。

#### 完整工程
1. push到master时触发
2. actions/checkout@v2用来拉取github仓库代码
3. set nodejs and npm install and npm run build
4. 推送到远端服务器
```
name: Node CI

on:
  push:
    branches:
      - master

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2

    - name: Install Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'

    - name: Install npm dependencies
      run: npm install

    - name: Run build task
      run: npm run build --if-present

    - name: Deploy to Server
      uses: easingthemes/ssh-deploy@v2.1.5
      env:
          SSH_PRIVATE_KEY: ${{ secrets.SERVER_SSH_KEY }}
          ARGS: "-rltgoDzvO --delete"
          SOURCE: "dist/"
          REMOTE_HOST: ${{ secrets.REMOTE_HOST }}
          REMOTE_USER: ${{ secrets.REMOTE_USER }}
          TARGET: ${{ secrets.REMOTE_TARGET }}
```

## CICD-自动测试和自动部署falsk后端
1. deploy.yml
	```
	name: backend CD

	on:
	push:
		branches:
		- master

	jobs:
	build-test:
		runs-on: ubuntu-latest
		steps:
		- uses: actions/checkout@v2
		- name: Pre run backend
			run: |
			python run.py
		
	deploy:
		runs-on: ubuntu-latest
		needs: build-test
		steps:
		- name: Deploy on Server
			uses: appleboy/ssh-action@master
			with:
			host: ${{ secrets.REMOTE_HOST }}
			username: ${{ secrets.REMOTE_USER }}
			key: ${{ secrets.SERVER_SSH_KEY }}
			script: |
				cd /home/ubuntu/mysports
				. venv/bin/activate
				cd backend
				git pull
				git checkout nightly
				git pull
				pip install -r requirements.txt
				sudo supervisorctl restart MySports
	```

2. test.yml
	```
	name: test

	on:
	push:
		branches:
		- nightly

	jobs:
	build-test:
		runs-on: ubuntu-latest
		steps:
		- uses: actions/checkout@v2
		- name: Pre run backend
			run: |
			pip install -r requirements.txt
			export FLASK_APP=run.py
			flask test
	```