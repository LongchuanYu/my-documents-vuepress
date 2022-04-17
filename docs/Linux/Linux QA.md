---
title: Linux QA
date: 2021-10-31 07:28:26
permalink: /pages/27dad8/
categories:
  - Linux
tags:
  - 
---
# QA
## Linux定时任务：crontab
[ 描述 ]  
每天18：30准时执行shell脚本
 
[ 解决方案 ]  
`vim /etc/crontab`或者`crontab -e`来创建新任务：
```
MAILTO=""
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
30 18   * * *   bash /home/xyz/auto.sh >> /home/xyz/auto_github_log.txt 2>&1
```
附上定时上传到github的脚本：  
```
#!bin/bash
thistime=$(date "+%m%d%H%M")
cd ~/Documents/my-documents
status=`git status`
case1="Changes to be committed"
case2="Changes not staged for commit"
echo "-------------$thistime-------------"
if [[ $status == *$case1* ]] || [[ $status == *$case2* ]]
then
  git add .
  git commit -m $thistime
  git push origin master
else
  echo "nothing to commit, working directory clean" 
fi
echo -e "\n"
```
[ 注意 ]  
1. **一定要加`bash`**，否则不会执行！！！刚开始用的是`sh`，但是`sh`不支持双括号`[[`的语法。
2. crontab文件顶头加`MAILTO=“”`，不然老是报错：`(CRON) info (No MTA installed, discarding output)`
3. 参考：https://ubuntuqa.com/article/407.html

## 以下命令有什么区别？
`source`,`sh`,`bash`,`.`,`./`
```
1. source：在当前shell执行，不需要执行权限
2. . ： 同source，是简写版

3. sh：在subshell中执行，不需要执行权限
4. bash：同sh，是sh的高级版 

5. ./ ： 在subshell中执行，需要执行权限
```
## Linux下查看大文件
1. 查看文件的前多少行
    ```
    head -10000 /var/lib/mysql/slowquery.log > temp.log
    
    上面命令的意思是：把slowquery.log文件前10000行的数据写入到temp.log文件中。
    ```
2. 查看文件的后多少行
    ```
    tail -10000 /var/lib/mysql/slowquery.log > temp.log
    
    上面命令的意思是：把slowquery.log文件后10000行的数据写入到temp.log文件中。
    ```
3. 查看文件的几行到几行
    ```
    sed -n '10,10000p' /var/lib/mysql/slowquery.log > temp.log
    
    上面命令的意思是：把slowquery.log文件第10到10000行的数据写入到temp.log文件中。
    ```
4. 根据查询条件导出
    ```
    cat catalina.log | grep   '2017-09-06 15:15:42' > test.log
    ```
5. 实时监控文件输出
    ```
    tail -f catalina.out
    ```
## kill pkill有什么区别？

## tree命令如何忽略查看文件夹？
```
tree -I "node_modules"
```
## ubuntu如何释放内存？
1. 释放内存
    ```
    sudo sh -c 'echo 1 > /proc/sys/vm/drop_caches'
    sudo sh -c 'echo 2 > /proc/sys/vm/drop_caches'
    sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
    ```
2. 查看内存
    ```
    free -m
    ```

## 管道运算符的用法
在terminal输入：aws configure后，会让用户输入id，key，地区。  
这里我们用管道运算符 “|” ， 把printf输出的数据作为aws configure的输入。
```
printf "$UPLOADER_AWS_S3_ID\n$UPLOADER_AWS_S3_KEY\n$AWS_S3_REGION\n\n" | aws configure
```

## terminal根据前缀通过上下键快速查找历史记录
```
在 ~/.inputrc(没有就创建一个)或/etc/inputrc 最后面加上如下代码
"\e[A": history-search-backward
"\e[B": history-search-forward
"\e[1~": beginning-of-line
"\e[4~": end-of-line
关闭terminal再打开即可生效
```

## terminal ctrl+左右键移动整个单词
```
在 ~/.inputrc(没有就创建一个)或/etc/inputrc 最后面加上如下代码
"\e[1;5C": forward-word
"\e[1;5D": backward-word
"\e[5C": forward-word
"\e[5D": backward-word
"\e\e[C": forward-word
"\e\e[D": backward-word
关闭terminal再打开即可生效
```

## 命令 cd - 是什么？
比如我们执行了一下的几个命令：
```
cd ~/Documents/
cd ~/Music/
```
这时候执行`cd -`，会切回到上一个地址： `～/Documents/`

## 在命令行中加入临时变量
我们用命令行来实现以下python代码：
```
name = "liyang"
print(name)
```
命令行：
```
NAME=liyang  // 设置临时变量，如果要设置临时环境变量用：export $NAME=liyang
echo $NAME  // liyang
```

## 彻底杀死uwsgi
今天出现了一件很神奇的事情，uwsgi杀死后又马上复活了，重启机器都没用
1. kill -9 pid  -- NG
2. killall ...  -- NG
3. pkill ...    -- NG
4. 直到：
```
登录到管理员账户： sudo su
打开htop
搜索uwsgi
f9
选择9： SIGKILL
回车
OK
```

然而后面又出现这种情况，用上面的方法无解，最后：  
把docker容器停掉，解


## 如何建立桌面快捷方式？
```
[Desktop Entry]
Version=1.0
Name=XYZ Studio
Exec=gnome-terminal -e "bash /home/xyz/studio_run.sh"
Comment=XYZ Studio
Icon=/home/xyz/Pictures/xyz_logo.png
Type=Application
Terminal=true
Encoding=UTF-8
Categories=Application;
```

## shell如何转换成二进制文件
```
shc
```

## ubuntu取消输入法ctrl+shift+F快捷键
1. 单击右上角输入法图标
2. 单击`Configure`
3. 切换到`Addon`选项卡
4. 找到`Simplified Chinese To Traditional Chinese`
5. 取消掉快捷键