---
title: 在本地调试bitbucket pipeline
date: 2021-10-31 13:17:55
permalink: /pages/cc9fca/
categories:
  - 部署&运维
tags:
  - 
---
## 在本地调试bitbucket pipeline
[ 环境准备 ]
1. 新建文件夹：docker_shell
2. 创建文件：docker_env
    ```
    BITBUCKET_CLONE_DIR=/opt/atlassian/pipelines/agent/build
    BITBUCKET_REPO_SLUG=xyz-data-uploader
    UPLOADER_AWS_S3_ID=xxxx
    UPLOADER_AWS_S3_KEY=xxxx
    AWS_S3_REGION=cn-northwest-1
    ```
3. 创建文件：docker_run.sh
    ```
    sudo docker run -it -v /home/xyz/xyz_app/catkin_ws/src/xyz-data-uploader:/opt/atlassian/pipelines/agent/build -v /home/xyz/docker_shell:/workspace -w /opt/atlassian/pipelines/agent/build --env-file=/home/xyz/docker_shell/docker_env xyzrobotics/ubuntu16.04:ros /bin/bash
    ```
4. 创建文件： docker_shell.sh
    ```
    # apt-get update && apt-get -y install zip net-tools
    pip install marshmallow==2.19.2 boto3==1.16.7
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /root/.cache/* && \
    rm -rf /var/cache/apt/archives/*.deb
    ./tools/code_style_check.sh
    mkdir -p /home/xyz/Documents
    mkdir -p /home/xyz/catkin_ws/src/$BITBUCKET_REPO_SLUG && source /opt/ros/kinetic/setup.bash
    cp -r ./* /home/xyz/catkin_ws/src/$BITBUCKET_REPO_SLUG
    cd /home/xyz/catkin_ws && catkin_make
    printf "$UPLOADER_AWS_S3_ID\n$UPLOADER_AWS_S3_KEY\n$AWS_S3_REGION\n\n" | aws configure
    source /home/xyz/catkin_ws/devel/setup.bash
    # Unit test
    aws s3 cp s3://xyz-packages/test/data_uploader/uploader_config.yml /home/xyz/uploader_config.yml
    roslaunch data_uploader main.launch upload_cg:=/home/xyz/uploader_config.yml use_pyc:=False use_thread:=True &
    sleep 10s && rosnode list
    python /home/xyz/catkin_ws/src/$BITBUCKET_REPO_SLUG/src/unit_test.py --config_path=/home/xyz/uploader_config.yml --use_pyc=False --use_pipeline=True
    ```
[ 步骤 ]
1. 在docker_shell下执行：
    ```
    sudo chmod +x docker_run.sh
    ./docker_run.sh

    此时在docker容器中挂载了两个目录。
    ```
2. 进入docker容器后，执行：
    ```
    source /workspace/docker_shell.sh
    ```

[ 问题 ]
1. catkin_make后rosrun data_uploader无效
    ```
    source /home/xyz/catkin_ws/devel/setup.sh   (错误)
    source /home/xyz/catkin_ws/devel/setup.bash (正确)
    ```

2. boto3.client(...)初始化之后，使用list_objects_v2提示access denie  
    原因：
    ```
    其实就是使用的key_id和access_key没有权限。。。。。换一个就好了
    ```
3. 
