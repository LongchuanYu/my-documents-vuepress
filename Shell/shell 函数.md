## 定义一个函数
```sh
function name() {
    statements
    [return value]
}


name() {
    statements
    [return value]
}

function name {
    statements
    [return value]
}
```

## 函数返回值
函数里面return语句返回的是函数的退出状态。  
状态是一个介于 0~255 之间的整数，其中只有 0 表示成功，其它值都表示失败。
如何得到函数的处理结果？
- 一种是借助全局变量，将得到的结果赋值给全局变量；
- 一种是在函数内部使用 echo、printf 命令将结果输出，在函数外部使用$()或者``捕获结果。