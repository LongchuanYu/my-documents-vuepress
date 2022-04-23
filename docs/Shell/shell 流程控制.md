## if 语句
#### 语法
基本
```sh
if condition
then
    statement
fi
```

if then同处一行，必须加分号
```sh
if  condition;  then
    statement(s)
fi 
```

if else
```sh
if  condition
then
   statement1
else
   statement2
fi


if  condition1
then
   statement1
elif condition2
then
    statement2
else
   statementn
fi
```

#### 和test命令结合
语法
```sh
if test expression; then
    statements
fi

# 或者

if []; then
    statements
fi
```

test文件判断
```
-b filename	判断文件是否存在，并且是否为块设备文件。
-c filename	判断文件是否存在，并且是否为字符设备文件。
-d filename	判断文件是否存在，并且是否为目录文件。
-e filename	判断文件是否存在。
-f filename	判断文件是否存在，井且是否为普通文件。
-L filename	判断文件是否存在，并且是否为符号链接文件。
-p filename	判断文件是否存在，并且是否为管道文件。
-s filename	判断文件是否存在，并且是否为非空。
-S filename	判断该文件是否存在，并且是否为套接字文件。

-r filename	判断文件是否存在，并且是否拥有读权限。
-w filename	判断文件是否存在，并且是否拥有写权限。
-x filename	判断文件是否存在，并且是否拥有执行权限。
-u filename	判断文件是否存在，并且是否拥有 SUID 权限。
-g filename	判断文件是否存在，并且是否拥有 SGID 权限。
-k filename	判断该文件是否存在，并且是否拥有 SBIT 权限。

filename1 -nt filename2	判断 filename1 的修改时间是否比 filename2 的新。
filename -ot filename2	判断 filename1 的修改时间是否比 filename2 的旧。
filename1 -ef filename2	判断 filename1 是否和 filename2 的 inode 号一致，
                        可以理解为两个文件是否为同一个文件。这个判断用于判断硬链接是很好的方法
```

test 数值判断
```
num1 -eq num2	判断 num1 是否和 num2 相等。
num1 -ne num2	判断 num1 是否和 num2 不相等。
num1 -gt num2	判断 num1 是否大于 num2 。
num1 -lt num2	判断 num1 是否小于 num2。
num1 -ge num2	判断 num1 是否大于等于 num2。
num1 -le num2	判断 num1 是否小于等于 num2。
```

test 字符串判断
```
-z str	        判断字符串 str 是否为空。
-n str	        判断宇符串 str 是否为非空。
str1 = str2
str1 == str2	=和==是等价的，都用来判断 str1 是否和 str2 相等。
str1 != str2	判断 str1 是否和 str2 不相等。
str1 \> str2	判断 str1 是否大于 str2。\>是>的转义字符，这样写是为了防止>被误认为成重定向运算符。
str1 \< str2	判断 str1 是否小于 str2。同样，\<也是转义字符。
```

test 逻辑表达式
```sh
expression1 -a expression	逻辑与
expression1 -o expression2	逻辑或
!expression	                逻辑非，对 expression 进行取反。

例：
if [ -z "$str1" -o -z "$str2" ]  #使用 -o 选项取代之前的 ||
then
    echo "字符串不能为空"
    exit 0
fi
```

#### [[]]的用法
[[ ]]是 Shell 内置关键字，它和 test 命令类似，也用来检测某个条件是否成立。

test 能做到的，[[ ]] 也能做到，而且 [[ ]] 做的更好；
test 做不到的，[[ ]] 还能做到。
可以认为 [[ ]] 是 test 的升级版，对细节进行了优化，并且扩展了一些功能。


## case in语句
```sh
printf "Input integer number: "
read num
case $num in
    1)
        echo "Monday"
        ;;
    2)
        echo "Tuesday"
        ;;
    3)
        echo "Wednesday"
        ;;
    4)
        echo "Thursday"
        ;;
    5)
        echo "Friday"
        ;;
    6)
        echo "Saturday"
        ;;
    7)
        echo "Sunday"
        ;;
    *)
        echo "error"
esac
```

## while循环
```sh
#!/bin/bash
i=1
sum=0
while ((i <= 100))
do
    ((sum += i))
    ((i++))
done
echo "The sum is: $sum"
```


## for循环

#### C风格
```
for((exp1; exp2; exp3))
do
    statements
done
```

#### python风格
```
for variable in value_list
do
    statements
done
```


## select
select in 循环用来增强交互性，它可以显示出带编号的菜单，用户输入不同的编号就可以选择不同的菜单，并执行不同的功能。

```sh
value_list=("eat" "sleep" "drink" "play")
select value in $value_list[*]
do
    echo $value
done

1) eat
2) sleep
3) drink
4) play
```