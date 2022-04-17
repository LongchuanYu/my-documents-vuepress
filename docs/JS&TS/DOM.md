---
title: DOM
date: 2021-10-31 13:35:04
permalink: /pages/650bed/
categories:
  - JS&TS
tags:
  - 
---
## html的Attribute和Dom的Property的区别
- Atribute：用来初始化DOM的Property  
- Property：可以随意改变  

很不错的一个例子：  
```
当浏览器渲染 <input type="text" value="Sarah"> 时，它会创建一个对应的 DOM 节点，其 value Property 已初始化为 “Sarah”。

<input type="text" value="Sarah">  

当用户在 <input> 中输入 Sally 时，DOM 元素的 value Property 将变为 Sally。  
但是，如果使用 input.getAttribute('value') 查看 HTML 的 Attribute value，则可以看到该 attribute 保持不变 —— 它返回了 Sarah。
HTML 的 value 这个 attribute 指定了初始值；  
DOM 的 value 这个 property 是当前值。
```

