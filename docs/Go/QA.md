## 尝试go的模块引入，报错
错误信息：
```
package pkg/a is not in GOROOT (/usr/local/go/src/pkg/a)
```

## 字符串出现“``”符号是什么意思
```
type Model struct {
	ID int `gorm:"primary_key" json:"id"`
	CreatedOn int `json:"created_on"`
	ModifiedOn int `json:"modified_on"`
}
```
解释：
```

解析：
使用 "" 包裹的字符串 会解析 字符串中的 转义符
使用 `` 包裹的字符串 不会解析 字符串中的 转义符

性能：
使用 "" 包裹的字符串 性能 比较慢 到 极慢（由 转义符 和 字符串的长度 决定）
使用 `` 包裹的字符串 性能 极快

说明：
字符串 实际上是 字符数组（学过 其他编程语言 的可能都知道，据我所知 GoLang 的书籍都 没讲过 或 没重点讲）
解析 会 遍历 整一个 字符串 (字符数组)，寻找可以 解析 的 转义符，不管 原先字符串里 有没有 转义符，都会 遍历一次
不解析 则会 直接输出

作者：白祤星
链接：https://www.jianshu.com/p/e3ce1a137bdf
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## Go导入本地包
使用go mod包管理

#### 目录结构和代码
test
|-- go.mod
|-- main.go
`-- util
    `-- say.go
---

go.mod
```
/* 通过命令 go mod init test 生成 */

module test

go 1.18
```

say.go
```
package util

import "fmt"

func Say(value string) {
    fmt.Println(value)
}
```

main.go
```
package main

 import (
     "fmt"

     "test/util"
 )

 func main() {
     fmt.Println("begin....")
     util.PrintA("hello world")
 }

```

#### 注意事项
1. say.go中的package名字最好和所在文件夹的名字相同，为util
2. say.go中的方法名首字母要大写，否则方法是私有方法
3. main.go中import的写法是(重要)：
    ```
    import "test/util"

    test 是go.mod中给出的module名
    util 是util/say.go中给出的package名
    ```


#### 扩展
如果say.go中的package名字和文件夹名字不同怎么处理呢，比如package名改为say
解决方法，在main.go中import这样写：
```
import say "test/util"
```

#### 总结
import "test/util" 只到达文件夹，比如util就是文件夹名
import say "test/util" 同理双引号里面只能到到文件夹，要引用文件夹下的包就在前面加入包名say

## 同一包下的文件是共通的
#### 描述
有如下目录结构
```
test
├── go.mod
├── main.go
└── util
    ├── a.go
    └── b.go


// a.go
package say

const (
    STATUS = "this is A"
)

// b.go
package say

import "fmt"

func SayA() {
    fmt.Println(STATUS)
}

// main.go
package main
import (
    "fmt"
    say "test/util"
)

func main() {
    say.SayA()  // "this is A"
}

```

#### 分析
在b.go中直接使用了a.go中的常量，因为b.go和a.go同属于同一个包。
这样的操作是允许的。
