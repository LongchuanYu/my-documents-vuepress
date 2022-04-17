---
title: shell实战
date: 2021-10-31 07:28:26
permalink: /pages/7e54cd/
categories:
  - Linux
tags:
  - 
---

## 搜索文本中的字符串，根据获取的行号进行整行替换
```shell
# get last searched row number
searched_row_number=$(grep -n "^alias xyz_server_id=*" world.txt | tail -1 | cut -d ":" -f 1)
insert_command='alias xyz_server_id=17'

if [ $searched_row_number ]; then
  sed -i "$searched_row_number c $insert_command" world.txt
else
  # append to last row
  echo $insert_command >> world.txt
fi

#----------------------
# grep -n   grep的结果显示行号
# tail -1   只显示最后一行
# sed -i "4c hello" world.txt   把第四行替换成hello
```