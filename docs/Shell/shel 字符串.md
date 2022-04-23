## 获取字符串长度
```
str="abcdefg"
echo ${#str}  # 7
```

## 字符串拼接/合并
```sh
name="liyang"
age="99"

echo $name$age  # liyang99
echo "$name $age"  # liyang 99
echo $name"hello"$age  # liyanghello99
echo ${name}and${age}end  # liyangand99end
```

## 字符串截取
#### 截取指定位置
```sh
name="hello world"
echo ${name: 6: 9}  # world
echo ${name: 6}  # world
```

#### 从指定字符（子字符串）开始截取
语法
```
1. 使用#截取右边字符
${string#*chars}

通俗来说是： 忽略掉chars左边的所有字符，只保留chars右边的所有字符

-----------
2. 使用%截取左边字符
${string%chars*}
通俗来说是： 忽略掉chars右边的所有字符，只保留chars左边的所有字符

3. ## 或 %% 表示贪婪的匹配最后一个chars
```

例子
```sh
url="http://localhost:4200"
echo ${url#*:}  # 4200
echo ${url%:*}  # http://localhost
```