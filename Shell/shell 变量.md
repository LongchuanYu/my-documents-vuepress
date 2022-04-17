

## 定义变量
```
1. 直接字符串： name='hello world'
2. 变量引用： name="$USER"
3. 命令引用： name=$(COMMAND)   # name=$(grep 'liyang' test.txt)
```

## 引用变量
```
1. "$name"：弱引用，$name会被替换成变量值 ->  "liyang"
2. '$name'：强引用， -> "$name"
3. echo ${name}  # liyang
```

## 只读变量
```
readonly name
```

## 删除变量
```
unset myUrl
```

## 变量作用域
```
变量的生效范围等标准划分变量类型

1. 普通变量：生效范围为当前shell进程；对当前shell之外的其它shell进程，包括当前shell的子shell进程均无效
2. 环境变量：生效范围为当前shell进程及其子进程
3. 本地变量：生效范围为当前shell进程中某代码片断，通常指函数
```
