---
title: React QA
date: 2021-10-31 13:42:36
permalink: /pages/4ec6ec/
categories:
  - React
tags:
  - 
---
## 如何理解this.props.children？
请参考link:https://zh-hans.reactjs.org/docs/glossary.html#propschildren
1. 父组件：
    ```js
    class Parent extends React.Component{
        render(){
            return (
                <div>
                    Parents..
                    <Child>
                        <span>name: liyang</span>
                        <span>age: 18</span>
                    </Child>
                </div>
            )
        }
    }
    ```

2. 子组件
    ```js
    class Child extends React.Component{
        constructor(props){
            super(props)
        }
        render(){
            return (
                <div>
                    Child....
                    {this.props.children}
                </div>
            )
        }
    }
    ```
3. 效果：
    ```
    Parents
    name: liyang
    age: 18
    ```

## dva的model中巧用es6的展开运算符
达到类似于assign函数的效果
```
const Model = {
  namespace: 'login',
  state: {
    status: undefined,
    username: 'xxx'
  },
  reducers: {
    changeLoginStatus(state, { payload }) {
      const newStatus = 'loginSucess';
      return { ...state, newStatus };
    }
  }
};
export default Model;
```
`changeLoginStatus`返回了新的state：
```
state: {
    status: 'loginSucess',
    username: 'xxx'
}
```

## dva connect 中巧用结构运算符
```
export default connect(({ login }) => ({
  ...login
}))(AvatarDropdown);

```
connect的参数 { login } 和 state 有什么区别？
```
我们假设store状态树里面有以下的命名空间：
login
study
...

打印state：
{
    @@dva: 0
    loading: {global: false, models: {…}, effects: {…}}
    login: {isLogin: true, username: undefined}
    router: {location: {…}, action: "POP"}
    study: {...}
}

可见state包含了全部的命名空间，但是我们只要login，怎么办呢？
于是用解构运算符：{ login }
如果login里面还有属性，在返回的时候可以用展开运算符： ...login

```

## React + axios 全局拦截401错误，然后调用dva model里面的logout，如何实现？
参考：https://blog.csdn.net/Snow_GX/article/details/111354734?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-4.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-4.control
```
import {getDvaApp} from 'umi'
...
...
axios.interceptors.response.use(function(response){
  return response
},function(error){
  const status = error.response.status;
  switch(status){
    case 401:
      getDvaApp()._store.dispatch({
        type: 'login/logout'
      })
    
  }
  return Promise.reject(error)
})
...
...
```

## dva　→　model　→　effects中的异步函数，如何拿到state中的值？
[ 描述 ]  
如下所示，希望能取到dateRange的值。
```
const Model = {
  namespace: 'visionData',
  state: {
    dateRange: [],
  },
  effects: {
    *onAdvancedSearch({ payload }, { call, put, select }) {
        const dateRange = yield select(state => state.visionData.dateRange);
        console.log(dateRange);
    }
  }
};
export default Model;
```
[ 解决方案 ]
```
// 一定不要忘了加上namespace名： visionData
const dateRange = yield select(state => state.visionData.dateRange);
```