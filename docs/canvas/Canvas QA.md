---
title: Canvas QA
date: 2021-10-31 13:03:36
permalink: /pages/14be4c/
categories:
  - canvas
tags:
  - 
---
## 对canvas中坐标的理解
canvas中的各种方法,比如`moveTo(x,y),lineTo(x,y)`  
这里的坐标指的是***相对于画布***的坐标,而不是相对于浏览器或者工作区,切记!!  

## 在Canvas中如何画一条直线？
```
function draw_line(ctx,start,end) {
    const edge = new Path2D()
    edge.moveTo(start.x, start.y)
    edge.lineTo(end.x, end.y)
    ctx,beginPath()
    ctx.stroke(edge)
}
```
`Path2D()`用来构建一系列路径，这些路径可以被`CanvasRenderingContext2D`对象使用
1. moveTo是移动画笔  
2. lineTo: 是从moveTo移动到lineTo
3. 把画好的边加到canvas上下文中，并画出来
 
## canvas的width和weight
1. canvas的大小一定要由属性的width和weight指出: 
    ```
     <canvas id="imageCanvas"
         width="700"
         height="600">
    </canvas>
    ```  
2. 如果用css来指定,图像会被拉伸 



## ctx.clearRect 使用须知
**[ 描述 ]**  
画布1用来展示图片  
我在画布2画图,然后调用画布2.clearRect . 结果画布1的图片也被清空了...  
**[ 分析 ]**  
知道为什么所有的东西都被清空了么?来看看代码:  
```
<script>
    var img = document.getElementById("imageCanvas");
    var draw = document.getElementById("drawingCanvas")
    var imgCtx = img.getContext('2d')
    var drawCtx = img.getContext('2d') //问题出现在这里!!!
    window.onload = function(e) {
        drawCtx.lineCap='round';
        drawCtx.strokeStyle = 'red'
        drawCtx.lineWidth = 1.6
        imgData.src = 'assets/1.jpg'
        imgData.onload = function() {
            imgCtx.drawImage(imgData,0,0,img.width,img.height)
        }
        draw.oncontextmenu = function(e) {
            e.preventDefault()
        }
    };
    draw.onmousedown = function(e) {
        drawCtx.clearRect(0, 0, draw.width, draw.height);

    };
</script>
``` 
**神tm, 把img的上下文赋值给drawCtx...**


**[ 备注 ]**  
当然,这个代码是需要重构的,例如`draw.onmousedown`,在图片还没加载之前,是会报错的..  

  
## 用canvas画线的逻辑思路
1. 我们定义了一个边集合（edgeList），和一个点集合（pointList）
2. 鼠标按下，把该点的坐标push到点集合
3. 再次按下鼠标就形成了一条边，把边push到边集合
4. 只要有动画的地方都把边集合重绘一遍
