---
title: css-QA
date: 2021-10-31 13:05:55
permalink: /pages/5022a4/
categories:
  - CSS
tags:
  - 
---

## 自定义input框
```
.input-wrapper {
    border: 1px solid #e0e4e8;
    border-radius: 5px;
    padding: 5px;
    width: 5rem;
}
input {
    border-style: none;
    border-left: 2px solid #3e66fb94;
    height: 1rem;
    font-size: 0.85rem;
    padding-left: 5px;
    width: 90%;
    &:focus {
    outline: none;
    }
}
```