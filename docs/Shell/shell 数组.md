## 定义数组
```sh
nums1=(1 2 3 4 5)  # 相当于[1,2,3,4,5]

nums2=(1 2 "hello world")  # 相当于[1,2,'hello world']

nums3=(1 2)
nums3[3]=99  # 1 2 99 相当于[1,2,None,99]
```

## 获取数组元素
```sh
nums=(1 2 3 4 5)
echo ${nums[0]}  # 1
echo ${nums[*]}  # 1 2 3 4 5
echo ${nums[@]}  # 1 2 3 4 5
```

## 获取数组长度
```sh
nums=(1 2 3 4 "hello world")

echo ${#nums[*]}  # 5
echo ${#nums[4]}  # 11 因为hello world有11个字符
```

## 删除数组或元素
```sh
nums=(1 2 3 4 5)
unset nums[1]  # 1 3 4 5
unset nums  #
```

## 数组拼接（合并两个数组）
拼接数组的思路是：先利用@或*，将数组扩展成列表，然后再合并到一起。
```sh
nums1=(1 2)
nums2=(3 4)

nums=(${nums1[*]} ${nums2[*]})  # 1 2 3 4
```

## 关联数组（类似于python字典）
#### 声明关联数组
```sh
declare -A color
color["red"]="#ff0000"
color["green"]="#00ff00"
color["blue"]="#0000ff"
```
或者
```sh
declare -A color=(["red"]="#ff0000", ["green"]="#00ff00", ["blue"]="#0000ff")
```

#### 访问关联数组元素
```sh
echo $(color["red"])  # #ff0000
```

#### 获取所有元素的key和value
获取所有的key
```sh
echo ${color[*]}  # red green blue
```
获取所有的value
```sh
echo ${!color[*]}  # #ff0000 #00ff00 #0000ff
```

#### 获取关联数组长度
```sh
echo ${#color[*]}  # 3
```

