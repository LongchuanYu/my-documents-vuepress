---
title: GIT命令详解
date: 2021-10-31 13:34:12
permalink: /pages/792f0d/
categories:
  - git
tags:
  - 
---
## git fetch
- 如果git fetch后面没有指定参数，则默认fetch所有的分支。  
- 从远程获取分支，并与本地分支合并  
    ```
    git fetch <远程主机名> <远程分支名>:<本地分支名>
    ```
- fetch默认是和HEAD指针指向的分支进行合并，为什么fetch再merge也可以呢？  
看文档：  
    ```
    When FETCH_HEAD (and no other commit) is specified, 
    the branches recorded in the .git/FETCH_HEAD file by the previous invocation of git fetch for merging are merged to the current branch.
    ```
    
## git stash
用于临时保存当前工作区，或者恢复。
注意点：  
- git stash不会存储新增的文件，如果有新增的文件要`git add .`一下。  
- git stash save 后，会清空你的工作区（工作区会回到改动之前的状态）  
    比如：改动了文件a后，执行git stash，此时工作区回到改动文件a之前的状态
    
[ 增 ]
```
git stash  //暂存当前工作区
git stash save "message"  //暂存当前且加上描述
```
[ 删 ]  
```
git stash drop [stash name]  //例：git drop stash@{0}
git stash clear  //清除所有
```
[ 查 ]
```
git stash list  //显示所有的stash
git stash show [stash name] //显示指定stash的改动，加上 -p 显示更详细信息，这个改动是基于创建stash的时候和创建stash之前
```
[ 应用 ]
```
git stash pop  //把缓存堆栈中的第一个stash删除，并将对应修改应用到当前的工作目录下
git stash apply [stash name]  //通过名字指定使用哪个stash，默认使用最近的stash（即stash@{0}）
```


## git checkout
1. 以xx分支为基础新建分支
   ```
   git checkout -b [new branch] [base branch]
   ```

## git merge
merge隐含的信息是：都与当前分支进行merge
1. 将分支dev合并到当前分支中，自动进行新的提交：
    ```
    $ git merge dev
    ```

