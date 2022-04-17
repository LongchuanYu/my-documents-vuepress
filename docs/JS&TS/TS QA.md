---
title: TS QA
date: 2021-10-31 13:35:04
permalink: /pages/4d9ca0/
categories:
  - JS&TS
tags:
  - 
---
## 类型注解如何给出复数个注解？  
    参考：
    ```
    open<T, D = any, R = any>(componentOrTemplateRef: ComponentType<T> | TemplateRef<T>,
              config?: MatDialogConfig<D>): MatDialogRef<T, R>
    ```
 
## 原来接口还可以继承其它接口
    ```
    export interface MethodToGetRobotPoseResp extends HttpCommResponse {
      data: MethodToGetRobotPose;
    }
    ```
