---
title: Python QA
date: 2021-10-31 13:38:25
permalink: /pages/37fbbc/
categories:
  - Python
tags:
  - 
---
## 如何优雅的处理字典键？
[ 描述 ]  
```
设字典：
dic = {
    'name': 'liyang',
    'age': 110,
    'married': False
}

问题1. 如果引用不存在的键会报错： dic['like'] 
问题2. 可以用 if dic.get('like', None): pass 来判断字典键的存在吗？
```
[ 解决方案-问题1 ]  
```
用dic.get('like', None)设置一个默认值，就不会报错了。
注意：如何处理默认值None请看问题2
```
[ 解决方案-问题2 ]  
```
不能！
如果我们想要判断字典键 - "married" 是否存在：
    if dic.get('married', None):
        print('exits')
    else:
        print('not exits')
实际结果： 'not exits'
但是键'married'确实是存在的啊，问题出现在'married'的value是bool类型

所以正确的方式是这样：
    if dic.get('married', None) is not None:
        print('exits')
    else:
        print('not exits')
```

## “==操作符” 和 “is操作符”的区别是什么？

- “==操作符”：比较两个对象的值是否相等
- “is操作符”：两个变量名是否精确的指向同一对象

举例说明：虽然都是值一样的数组 [1,2,3]，但是 L 和 M 是两个不同的对象。

```
L = [1,2,3]
M = [1,2,3]
Print( L == M ) # True
Print( L is M )  # False
```



## Sort和Sorted的区别是什么

- Sort() ：List的内置排序方法，**原地排序**
- Sorted()：接受任何序列对象，**需要赋值给新的变量**

 

## 深拷贝和浅拷贝

####  ● 浅拷贝

```python
L1 = [1,2,3]
L2 = L1
L1[0] = 24

print(L1)		# [24,2,3]
print(L2)		# [24,2,3]
```

记住一句话：**赋值生成引用，而不是拷贝。**

因此这里修改了L1，L2也会变化。

#### ● 深拷贝

```python
L1 = [1,2,3]
L2 = L1[:]
L1[0] = 24

print(L1)		# [24,2,3]
print(L2)		# [1,2,3]
```

## 如何读写大文件（文件太大不能一次性读取）？


## python除法
1. 保留3位小数
```
ret = 596 / 1000.000
```
2. --
```
a=45
b=1420
ret = a/b * 100.0000` 结果为 `0.0`,如何解决？
答：float(a)/float(b) * 100.0
```

## 如何解决类中的冗余代码？
[ 描述 ]

如下，计算a和b的加法和乘法，同时a,b要求大于0
这样会导致在加法中写一次判断，在乘法中也写一次判断
怎么才是pythonsonic的写法呢
```
class Calculate:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def add(self):
        if self.a <= 0 or self.b <= 0:
            # 希望两个值都大于0
            return
        print(self.a + self.b)
    
    def multi(self):
        if self.a <= 0 or self.b <= 0:
            # 希望两个值都大于0
            return
        print(self.a * self.b)
```

## @staticmethod、@classmethod、@property的区别？

## python 输出到terminal的配色方案
link：https://pypi.org/project/termcolor/

## 字典是否支持dict.add(set([1,2,3,4,5]))?


## python format 转义
[ 描述 ]
```
template = '{"server_id": {server_id}}'.format(server_id=1500)
报错：KeyError: '"server_id"'
```

[ 解决方案 ]
```
'{' -> '{{'
'}' -> '}}'

template = '{{"server_id": {server_id}}}'.format(server_id=1500)
```

## python进程池不能调用类函数，很奇怪

## 奇怪的写法： if else 之间嵌套了一个for循环
输出：
1
hello
2
```
s = 1
if s == 1:
    print('1')
for i in range(1):
    print('hello')
else:
    print('2')

```

## 获取当前正在执行的函数名（inspect的使用）
```py
import  inspect
def test_fun():
    print(inspect.currentframe().f_code.co_name)  # test_fun
```

## python try..except..else 和 try..except..finally的区别
try..except..else
```
try...except捕获到异常才会去执行else
```

try..except..finally
```
不管try..except有没有捕获到异常，都会去执行finally
```

## python如何强行调用私有方法？
假设有如下类：
```py
class Test():
    def __init__(self):
        pass

    def __showMsg(self):
        print('hello')
```

直接调用会报错：
```py
test = Test()
test.__showMsg()  # AttributeError: 'Test' object has no attribute '__show'
```

其实python底层会将双下划线“__”的方法偷偷改名,   
比如 __showMsg 改成了 _Test__showMsg，  
所以只需要这样调用：
```py
test = Test()
test._Test_showMsg()  # hello
```

## zip的应用
方便的组合成字典：
```py
a = ['a', 'b']
b = [{'name': 'ba'}, {'name': 'bb'}]
print( dict(zip(a, b)) )
# {'a': {'name': 'ba'}, 'b': {'name': 'bb'}}
```