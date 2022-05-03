---
title: 常用的git alias
date: 2022-04-17 16:30:18
permalink: /pages/1d311e/
categories:
  - git
tags:
  - 
---
## 常用的git alias
```
alias gs='git status'
alias gb='git branch'
alias gc='git checkout .'
alias gf='git diff'
alias gcm='git checkout master'
alias gcn='git checkout nightly'
alias gc_feature='git branch|grep feature|xargs git branch -D'
alias gc_hotfix='git branch|grep hotfix|xargs git branch -D'
alias gac='git_add_and_commit() { git add . && git commit -m "$1"; }; git_add_and_commit'

```