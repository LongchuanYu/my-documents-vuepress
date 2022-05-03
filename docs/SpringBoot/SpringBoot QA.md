---
title: SpringBoot QA
date: 2022-04-17 16:30:18
permalink: /pages/3a7e84/
categories:
  - SpringBoot
tags:
  - 
---
## 创建了一个demo，写了启动类和controller，访问路由的时候报错：`Whitelabel Error Page`
#### [ 解决方案 ]
1. 因为我要返回字符串，所以controller里面要加上@ResponseBody注解。否则会返回一个view。

#### [ 原因分析 ]
启动类
```
...略

@SpringBootApplication
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}

```

controller
```java
/* 如果不加@ResponseBody注解，controller返回index，就会去找视图层的index.jsp，找不到就报错了。
*/
package com.example.demo.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/jsp")
    public String returnJspView(){
        return "index";
    }
}
```