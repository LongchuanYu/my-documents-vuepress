---
title: cpp-QA
date: 2021-10-31 13:05:45
permalink: /pages/ac48e6/
categories:
  - C++
tags:
  - 
---
## 使用yaml-cpp报错： undefined reference to ...
```
这样编译：
g++ -L/usr/local/lib/ -I/usr/include -o test t_cpp.cpp -lyaml-cpp

切记 -lyaml-cpp 一定要放在最后！
```