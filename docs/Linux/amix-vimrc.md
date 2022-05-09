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
```

