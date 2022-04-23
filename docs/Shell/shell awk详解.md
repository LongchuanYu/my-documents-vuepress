## awk是什么
1. awk是一个强大的文本分析工具，相对于grep的查找，sed的编辑，awk在其对数据分析并生成报告时，显得尤为强大。  
2. 简单来说awk就是把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理。  
3. 通常，awk是以文件的一行为处理单位的。awk每接收文件的一行，然后执行相应的命令，来处理文本。

## 语法
```
awk [option] 'pattern{action}' file1,file2,...filen

pattern：表示 AWK 在数据中查找的内容（找到pattern匹配的那一行，并执行action）
action：是在找到匹配内容时所执行的一系列命令，常用的命令：print
```

## pattern详解
pattern 表示 AWK 在数据中查找的内容（找到pattern匹配的那一行，并执行action），  
如果不加pattern，则每一行都会被匹配，  
pattern可以是：
1. 正则表达式
    ```
    语法： awk '/regex pattern/ { action }' file

    例子： 
    $ cat test.txt
    athis is test row 1
    bthis is test row 0.5
    cthis is test row 0

    $ cat test.txt | awk '/0.5/ { print $1 }' test.txt
    bthis

    例子解释： 找到 test.txt中包含“0.5”的行，并输出以默认分隔符分割后的第一个元素
    ```
2. 关系表达式
    ```
    语法： awk '参数 表达式 /正则/ { action }'

    例子：
    $ cat test.txt
    athis is test row 1
    bthis is test row 0.5
    cthis is test row 0

    $ cat test.txt | awk '$5 ~ /0.5/ {print $1}'
    bthis

    例子解释：
    找到 分割后 第5个元素 包含 0.5 的 所有行， 并输出第一个元素，
    ～符号表示包含
    !~表示不包含
    更多运算符请百度。
    ```
3. 范围  
    ```
    说明： awk搜索到两个pattern之间的行（包括两个pattern）

    语法： awk 'pattern1,pattern2 { action }' file

    例子1：
    $ cat test.txt
    athis is test row 1
    bthis is test row 0.5
    cthis is test row 0.3
    dthis is test row 0

    $ cat test.txt | awk '/bthis/,/dthis/ {print $5}'
    0.5
    0.3
    0

    例子1解释： 
    awk会先找到正则匹配到的bthis所在的行，
    再找到正则匹配到的dthis所在的行，
    然后返回这两行之间的内容，
    最后输出每行的第5个元素

    例子2：
    $ cat test.txt
    athis is test row 1
    bthis is test row 0.5
    cthis is test row 0.3
    dthis is test row 0

    $ cat test.txt | awk '$5 == 1,$5 == 0.3 {print $1}'
    athis
    bthis
    cthis

    例子2解释：
    同上，只不过范围不是正则，而是关系表达式了
    ```
4. 特殊表达式
    ```
    说明： 包含两个特殊表达式 BEGIN 和 END，
    用于在输出的前面一行，和后面一行添加内容

    例子：
    $ cat test.txt
    athis is test row 1

    $ cat test.txt | awk 'BEGIN { print "Start." }; { print $1 }; END { print "End." }'
    Start.
    athis is test row 1
    End.
    ```

5. 多个关系表达式相结合
    ```
    例子：
    $ cat test.txt
    athis is test row 1
    bthis is test row 0.5
    cthis is test row 0.3
    dthis is test row 0

    $ cat test.txt | awk '$5 > 0.3 && $5 < 1 {print $1}'
    bthis
    ```

## action 详解
action由大括号{}包围，当pattern匹配时执行。
可以有0个或多个action，复数个action由分号隔开，顺序执行。
action可以有：
1. 表达式
    ```
    比如变量赋值、算数运算符、加减操作等等
    ```
2. 控制语句
    ```
    if, for, while, switch, and more
    ```
3. 输出语句
    ```
    print或printf
    ```
4. 复合语句： 以上几种的组合
5. 输入语句：控制输入的处理
6. 删除语句：删除数组元素

## 内置变量
TODO

## 其他例子
#### 1. 过滤出IP地址
```
echo "http://192.168.1.37:8080/api" | awk '{sub(/:.*/,x)} 1'

结果： http://192.168.1.37
```

解释： 
```
我们把语句拆成几个部分
1. awk
略

2. {sub(/:.*/,x)}
这是一个action，省略了pattern。它的作用是把匹配到的正则替换成x
x是个没有定义的变量，所以等于是删除正则匹配到的内容，相当于 sub(/:.*/, "")

3. 1
这是一个pattern，省略了action。
awk如果省略了action，默认会执行print，
这里pattern是1，代表条件是true，会永远执行action，也就是print
```

总结：
```
1. awk支持多个pattern-action对：
---------------------------
Most often, each line in an awk program is a separate statement or separate rule, like this:

awk '/12/  { print $0 }
     /21/  { print $0 }' mail-list inventory-shipped
---------------------------

以下例子会先过滤出第一行，把1替换成999，再过滤出包含999的行进行打印，
说明前一段action会对后面的语句产生影响：
$ cat test.txt
athis is test row 1
bthis is test row 0.5
cthis is test row 0.3
dthis is test row 0

$ cat test.txt | awk '/1/ {sub(/1/, "999")} /999/ {print}'
athis is test row 999
```

## refer
https://linuxize.com/post/awk-command/