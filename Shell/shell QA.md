---
title: shell QA
date: 2021-10-31 07:28:26
permalink: /pages/f06d12/
categories:
  - Linux
tags:
  - 
---
## shell 中 -eq -ne -gt -lt ge le
```
-eq    //等于
-ne    //不等于
-gt    //大于
-lt    //小于
ge     //大于等于
le     //小于等于
```

## Shell $0, $#, $*, $@, $?, $$，$!和命令行参数的使用
```
$0	当前脚本的文件名
$n	传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。
$#	传递给脚本或函数的参数个数。
$*	传递给脚本或函数的所有参数。
$@	传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。
$?	上个命令的退出状态，或函数的返回值。
$$	当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。
$!  Shell最后运行的后台Process的PID
```

## shell 中的 set -e 和 set +e的区别
```
区别：

set -e ： 执行的时候如果出现了返回值为非零，整个脚本 就会立即退出 

set +e： 执行的时候如果出现了返回值为非零将会继续执行下面的脚本 
```

## find -print0和xargs -0原理及用法
平常我们经常把find和xargs搭配使用，例如：

find . -name "*.txt" | xargs rm
但是这个命令如果遇到文件名里有空格或者换行符，就会出错。因为xargs识别字符段的标识是空格或者换行符，所以如果一个文件名里有空格或者换行符，xargs就会把它识别成两个字符串，自然就出错了。

这时候就需要-print0和-0了。

find -print0表示在find的每一个结果之后加一个NULL字符，而不是默认加一个换行符。find的默认在每一个结果后加一个'\n'，所以输出结果是一行一行的。当使用了-print0之后，就变成一行了

```bash
(啊啊啊啊) xyz@xyz:~/ly_app $ find . -name "*.py" -type f -print0 
./1.py./4.py./3.py./2.py(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ 
(啊啊啊啊) xyz@xyz:~/ly_app $ find . -name "*.py" -type f -print0 | xargs -0 
./1.py ./4.py ./3.py ./2.py
(啊啊啊啊) xyz@xyz:~/ly_app $ 
```

然后xargs -0表示xargs用NULL来作为分隔符。这样前后搭配就不会出现空格和换行符的错误了。选择NULL做分隔符，是因为一般编程语言把NULL作为字符串结束的标志，所以文件名不可能以NULL结尾，这样确保万无一失。

所以比较我们推荐的比较保险的命令是

## $*和$@的区别
#### 相同点
```
$* 和 $@ 都表示传递给函数或脚本的所有参数
```
#### 不同点
不被双引号包围时：
```
当 $* 和 $@ 不被双引号" "包围时，它们之间没有任何区别，都是将接收到的每个参数看做一份数据，彼此之间以空格来分隔。
```

有双引号包围时：
- "$*"会将所有的参数从整体上看做一份数据，而不是把每个参数都看做一份数据。  
- "$@"仍然将每个参数都看作一份数据，彼此之间是独立的。  

比如传递了 5 个参数，那么对于"$*"来说，这 5 个参数会合并到一起形成一份数据，它们之间是无法分割的；  

而对于"$@"来说，这 5 个参数是相互独立的，它们是 5 份数据。  

如果使用 echo 直接输出"$*"和"$@"做对比，是看不出区别的；但如果使用 for 循环来逐个输出数据，立即就能看出区别来。  

比如：
```
#!/bin/bash
echo "print each param from \"\$*\""
for var in "$*"
do
    echo "$var"
done
echo "print each param from \"\$@\""
for var in "$@"
do
    echo "$var"
done
```
结果：
```
print each param from "$*"
a b c d
print each param from "$@"
a
b
c
d
```