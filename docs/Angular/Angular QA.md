---
title: Angular QA
date: 2021-10-31 12:54:18
permalink: /pages/b77a75/
categories:
  - Angular
tags:
  - 
---
## Material组件可以作为属性包含在div里面么？  -  
```
<div mat-dialog-content>
</div>
```

<br>

## 子组件在 constructor 里面打印父组件传过来的值，结果为空？  +  
不要卸载`constructor`里面，写在`ngOnit`里面

<br>

# 对 @input() 中 get 和 set 的理解
- set： 父组件 ---> 子组件，对传递的值进行处理
- get： 子组件 ---> View，视图引用该变量的时候进行处理。

<br>

## Angular 里面的标签为什么可以这么写？ -  
```
<div fxLayout="row" fxLayoutAlign="space-between center" class="camera-image-type-label">
</div>
```

## DomSanitizer是什么？
有如下代码：
```
constructor(private domSanitizer: DomSanitizer) { }
...
...
this.StreamImage = this.domSanitizer.bypassSecurityTrustUrl(
          'data:image/jpg;base64,' + stream.image);
```
详见https://angular.cn/api/platform-browser/DomSanitizer  
主要作用是返回安全的HTML内容

## 弹出窗口的关闭
`dialogRef.afterClosed()`仅当按下确认按钮时触发  
点击mask或者取消来关闭都不会触发。。

## Angular styles.scss权重问题
[ 描述 ]
```
预期效果是，把global css全写在styles.scss里面，html方便调用。
结果发现，styles.scss的样式权重不高，很容易会被覆盖。
```
[ 解决方案 ]
```
还是得用import的方式，这样权重高一点。styles.scss里面只写。。
```

## 可观察对象的用法
```js
getRosHandler(): Observable<any> {
  if (this.isRosHandlerInit === false) {
    this.rosHandler = new ROSLIB.Ros({
      url: 'ws://' + window.location.hostname + ':9191'
    });
    return new Observable<any>(observer => {
      this.rosHandler.on('error', () => {
        this.isRosHandlerInit = false;
        observer.error(undefined);
      });
      this.rosHandler.on('connection', () => {
        this.isRosHandlerInit = true;
        observer.next(this.rosHandler);
      });
    });
  } else {
    return new Observable<any>(observer => {
      observer.next(this.rosHandler);
    });
  }
}


this.ros.getRosHandler().subscribe((rosHandler: ROSLIB.Ros) => {})
```

## 可观察对象，返回unsubscribe()
```
onWebSockEvent(event: string): Observable<any> {
  return new Observable<string>(observer => {
    this.webSock.on(event, (data: any) => {
      observer.next(data);
    });
    const listeners = this.webSock.listeners(event);
    const listener = listeners[listeners.length - 1];
    const that = this;
    return {
      unsubscribe(): void {
        that.webSock.removeListener(event, listener);
      }
    };
  });
}
```

## ngfor 切片
```
<div *ngFor="let pose of robotPoseFormTouch[curPoseFormStep]|slice:0:3; let index=index;"</div>
```

## 方法装饰器
```
export const initialPoseCheck = () => {
  return (target: object, methodName: string, descriptor: PropertyDescriptor) => {
    const oldMethod = descriptor.value;
    descriptor.value = function(...args: any): any {
      if (!this.initialRobotPoseArr?.length) {
        this.utilsService.showMessage.warn('请先注册初始位姿');
        // 不通过就return null，不会执行方法
        return null;
      }
      // 执行方法
      return oldMethod.apply(this, args);
    };
    return descriptor;
  };
};
```