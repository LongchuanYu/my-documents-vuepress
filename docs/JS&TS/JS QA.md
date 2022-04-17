---
title: JS QA
date: 2021-10-31 13:35:04
permalink: /pages/2e5797/
categories:
  - JS&TS
tags:
  - 
---
## One Word
1. js数组里面不能放元组  
2. as 表达式表示类型断言，另外的类型断言还有\<T>这种形式
3. Ts 的 “?.” 表示安全引用属性
4. js的函数传参是真的麻烦，特别是传不定参数的时候，要以对象的形式传。。。


## 如何理解click.event ?
#### ● event.clientX , event.clientY
- ![](../.vuepress/public/assets/html_offset.jpeg)   
设黄色区域为`divImg`,则:  
`divImg.offsetLeft / divImg.offsetTop` 是图片相对于父容器坐标的位置,注意不是event属性.


#### ● event.button
- 0:左键  
- 1:中键  
- 2:右键

## 前后两次console.log的结果一样，明明中间的值有所改动，为什么呢？
[ 问题描述 ]
```javascript
var person={
	name:'liyang'
}
console.log(person); //点开三角后-person:{name:'liyang',age:18}
person.age=18;
console.log(person); //点开三角后-person:{name:'liyang',age:18}
```
[ 答 ] 
```  
在用console.log()输出数组或对象时，对象（数组）的属性有时是折叠不显示的。  
如果你在console.log()输出之后改变了对象（数组）的属性，当你点击三角展开属性时，会重新读取对象（数组）当前的属性，也就是显示出来的是你点击三角展开时的属性，而不是执行console.log()时的属性。
```

## 箭头函数和function的区别?
- 箭头函数: this指向定义函数的环境
- Function: 根据环境的变化而变化


## apply, call, bind的区别？
1. 在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。
	```js
	func.call(this, arg1, arg2);
	func.apply(this, [arg1, arg2])
	//其中 this 是你想指定的上下文，他可以是任何一个 JavaScript 对象(JavaScript 中一切皆对象)，call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。　　
	```  
2. bind
	```
	在讨论bind()方法之前我们先来看一道题目：
	var altwrite = document.write;
	altwrite("hello");
	结果：Uncaught TypeError: Illegal invocation
	altwrite()函数改变this的指向global或window对象，导致执行时提示非法调用异常，正确的方案就是使用bind()方法：
	altwrite.bind(document)("hello")
	当然也可以使用call()方法：
	altwrite.call(document, "hello")
	```

## js中如何优雅的处理一些属性不存在的错误？

## js如何判断 err = {} 这个对象是否为空
没有原生的api可以用，需要曲线救国

## js解码jwt生成的Token
```js
let token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImxpeWFuZyIsImlhdCI6MTYwOTY2MzA2NCwidXNlcl9pZCI6MSwiZXhwIjoxNjA5NjkxODY0fQ.Y5WTWuc9zV2WtP6CqkWrexPpnxJ4IdeSmafPKWe4BLk";
let [header, payload, signature] = token.split('.');
console.log(atob(payload))  // {"username":"liyang","iat":1609663064,"user_id":1,"exp":1609691864}
```

## 快速取得对象数组指定属性列表
```
var data = [{
        a: 1,
        b: 2,
        c: 3
    }, {
        a: 4,
        b: 5,
        c: 6
    }, {
        a: 7,
        b: 8,
        c: 9
}];
let ret = data.map(item => item.a)
console.log(data)  // [1, 4, 7]
```

## 一行创建二维数组，且包含对象，且对象都是深拷贝
```
public poseForm = Array.from({length: 5}, () => [
	{！！
		label: 'x',
		value: 0,
		validator: new FormControl(null, mmControlParams),
	}, {
		label: 'y',
		value: 0,
		validator: new FormControl(null, mmControlParams),
	}, {
		label: 'z',
		value: 0,
		validator: new FormControl(null, mmControlParams),
	}, {
		label: 'r',
		value: 0,
		validator: new FormControl(null, angleControlParams),
	}, {
		label: 'p',
		value: 0,
		validator: new FormControl(null, angleControlParams),
	}, {
		label: 'q',
		value: 0,
		validator: new FormControl(null, angleControlParams),
	}]
);
```

## js两个感叹号含义
```
强制转换为bool值，比如判断数组是否有值： !!data.robotJoints.length
```


## && 的妙用
```
onRemove(): void {
	const layerElement = document.getElementById(this.domId);
	layerElement && layerElement.remove();
}
```