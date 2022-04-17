---
title: Docker QA
date: 2021-10-31 13:17:55
permalink: /pages/efc463/
categories:
  - 部署&运维
tags:
  - 
---
## docker 报错 Permission denied
```
1. 添加docker用户组
sudo groupadd docker

2. 将登陆用户加入到docker用户组中
sudo gpasswd -a $USER docker	#USER处是你自己的用户名

3. 更新用户组
newgrp docker
```

## 进入已经运行的docker容器
```
sudo docker exec -it a8e5731d23f5 /bin/bash 
```

## docker容器和主机互相拷贝数据
1. docker容器 -> 主机
    ```
    docker cp <containerId>:/file/path/within/container /host/path/target
    ```

## docker挂载宿主目录
```
docker run -it -v /test:soft centos /bin/bash
```

## 进入容器时加载环境变量
1. 方式一：打包设置dockerfile
    ```
    在通过Dockerfile打包镜像的时候可以配置环境变量：
    ENV SERVER_PORT 80 
    ENV APP_NAME pkslow 
    ```
2.  方式二：启动设置docker run --env
    ```
    使用--env和-e是一样效果的，示例如下：
    $ docker run -itd --name=centos -e SERVER_PORT=80 --env APP_NAME=pkslow centos:7 
    b3d42726ca6cdddd7ae09d70e720d6db94ff030617c7ba5f58374ec43f8e8d68 
    
    $ docker exec centos env 
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin 
    HOSTNAME=b3d42726ca6c 
    SERVER_PORT=80 
    APP_NAME=pkslow 
    HOME=/root 
    ```

3. 方式三：启动时加载文件docker run --env-file
    ```
    先把配置信息放在文件env.list里：
    $ cat env.list  
    VAR1=www 
    VAR2=pkslow.com 
    VAR3=www.pkslow.com 

    启动容器时传入文件：
    $ docker run -itd --name=centos --env-file env.list centos:7 
    1ef776e2ca2e4d3f8cdb816d3a059206fc9381db58d7290ef69301472f9b4186 
    
    $ docker exec centos env 
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin 
    HOSTNAME=1ef776e2ca2e 
    VAR1=www 
    VAR2=pkslow.com 
    VAR3=www.pkslow.com 
    HOME=/root 
    ```

## 进入容器后运行脚本
挂载了两个目录。
注意： 运行脚本结束后会退出docker。。
```
docker run -it 
-v /home/xyz/xyz_app/catkin_ws/src/xyz-data-uploader:/opt/atlassian/pipelines/agent/build 
-v /home/xyz/docker_shell:/workspace 
xyzrobotics/ubuntu16.04:ros 
/bin/bash /workspace/docker_shell.sh
```

## 用脚本启动docker容器
启动前先判断旧容器是否存在，如果存在则删除旧容器
```
 CONTAINER_NAME=xxx
 IMAGE_NAME=zzz
 if [ "$(docker ps -aq -f status=exited -f status=created -f name=$CONTAINER_NAME)" ]; then
     # cleanup
     docker rm $CONTAINER_NAME
 fi

docker run -it --name $CONTAINER_NAME $IMAGE_NAME
```

## 将镜像导出，并在其他电脑上导入（实现复制镜像的功能）
#### 导出镜像
```
docker save ubuntu:14.04 > /images/ubuntu_14.04.tar
```
#### 导入镜像
```
docker load --input  /images/ubuntu_14.04.tar
```
或者
```
docker load < /images/ubuntu_14.04.tar
```