
## 类变量（类属性）
#### 什么是类变量
```py
class Test:
    # 类变量： eat
    eat = ''
    def __init__(self) -> None:
        pass
```

#### 特点
1. 类的实例化对象都同时共享类变量，也就是说，类变量在所有实例化对象中是作为公用资源存在的。（可以用来存一些公共变量）
2. 类方法的调用方式有 2 种，既可以使用类名直接调用，也可以使用类的实例化对象调用。
```py
class Test:
    eat = '0'
    def __init__(self) -> None:
        pass

# 初始值： 0
print(Test.eat)  # '0'

# 实例化后访问也是： 0
a = Test()
print(a.eat)  # '0'

# 改变类变量，会影响到所有实例化的对象
a = Test()
b = Test()
Test.eat = 'banana'  # 改变！
print(a.eat)  # 'banana'
print(b.eat)  # 'banana'

# 如果改变实例里面的eat，会发生什么呢
# 结论是： 实例里面的eat发生改变，而类变量仍然不变
c = Test()
c.eat = 'shit'
print(c.eat)    # 'shit'
print(Test.eat) # 'banana'
```

## 实例变量（实例属性）
#### 什么是实例变量
```py
class Test:
    def __init__(self) -> None:
        # 实例变量：say
        self.say = ''

    def out(self):
        print(self.say)
```
#### 特点
1. 只作用于实例化的对象
    ```py
    """
    如下可见实例a改变了say的值，而实例b并不受到影响
    """
    a = Test()
    a.say = 'hello'
    a.out()  # 'hello'

    b = Test()
    b.out()  # ''
    ```
2. 实例变量只能通过对象名访问，无法通过类名访问。