---
title: Ros QA
date: 2021-10-31 13:43:42
permalink: /pages/e17fda/
categories:
  - ROS
tags:
  - 
---
## Topic和Service的区别
1. Topic是基于发布/订阅  
    data type：Messages（msg）
2. Service基于请求/响应  
    data type：（srv）

|-|rospy|C++|
|---|---|---|
|**Topic-Publish**|`rospy.Publisher('chatter', String, queue_size=10)`|`nh.advertise(...)`|
|**Topic-subscribe**|`rospy.Subscriber("chatter", String, callback)`|`nh.subscribe(...)`|
|**Service-server**|`rospy.Service('add_two_ints', AddTwoInts, handle_add_two_ints)`|`n.advertiseService("add_two_ints", add);`|
|**Service-client**|`add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)` <br> `resp1 = add_two_ints(x, y)`|` n.serviceClient(...);`|  
  


## rospy.init_node('node_name')有什么作用？

## rospy.spin()有什么作用？
1. 假设有如下配置（main.launch）
    ```
    ...
    <node pkg="data_uploader" type="have_spin.py" name="data_queue_handler" output="screen"/>
    <node pkg="data_uploader" type="no_spin.py" name="data_queue_handler" output="screen"/>
    ...
    ```
2. 用配置文件启动该package
    ```
    roslaunch data_uploader main.launch
    ```
3. 结论
    ```
    1. 有rospy.spin()的节点会一直运行
    2. 没有的则退出： [log_uploader-3] process has finished cleanly
       如果不想加入rospy.spin()，又不想这个节点退出，可以写个线程让这个节点在后台运行
    ```

## 报错：roslaunch error: ERROR: cannot launch node of type
```
1）第一个最常见的错误就是没有source，我不是这个错误，如果有类似问题的同学可以先试下运行

source devel/setup.bash

2）第二个错误比较蠢，我自己有一次犯傻了，git了一个包之后没有make就开始运行，检查了半天最后才意识到。。。。

3）最后一个常见的问题，在git clone之后，其中的部分文件失去了执行权限，比如这个controller.py，因此它没法生成可执行文件，启动文件就报错了。

解决办法很简单，改一下权限就可以了。

sudo chmod +x controller.py

之后编译重新运行就可以了。
```