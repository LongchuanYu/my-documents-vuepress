## QA
#### 如何在NERDTree中新建文件
```
输入ma
```
#### 如何在vim中打开terminal，路径为当前打开的文件
```

```
#### 如何进行全文件搜索
```

```
#### split屏幕后如何调整大小
```

```
#### leader + o打开buffer如何关闭buffer
```
D
```
#### 如何在两个buffer之间快速跳转
```
1. <leader> b 打开buffer（此时光标只想上一个buffer）
2. 回车
```
#### NERDTree如何递归搜索文件名（搜索折叠目录） 
```
ctrl+f，搜索CWD下的所有文件
```
#### html标签如何自动补全
```

```
#### 如何安装到全局
按照官网说明安装，执行到`sh /opt/vim_runtime/install_awesome_parameterized.sh /opt/vim_runtime ubuntu lighthouse`的时候会报错：
```
/opt/vim_runtime/install_awesome_parameterized.sh: 34: Bad substitution
```
解决方案：
```
将/usr/bin/sh的软连接改为bash

$ dpkg-reconfigure dash

选择NO
```
原因：
```
发现sh链接的是dash
#### 如何刷新当前buffer（当前文件在别处被更新的情况）
```

```

#### 如何在NERVTree中显示当前打开文件
```

```
#### 切换NERDTree路径到CWD路径下
```
1. 如果在文件中，输入<leader> cd，切换CWD为当前文件所在的目录；如果实在NERDTree中则跳转到自己的工程目录下输入cd进行切换
2. 输入<leader> CD，将NERDTree路径切换到CWD下
```
