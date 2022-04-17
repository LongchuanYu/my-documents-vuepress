---
title: Flask QA
date: 2021-10-31 13:11:44
permalink: /pages/0fe02b/
categories:
  - Django&Flask
tags:
  - 
---
## flask流内容
```
def send_stream(args):
	# read file stream
	def send_file():
	    with open(zipfile_path, 'rb') as targetfile:
	        while 1:
	            data = targetfile.read(20 * 1024 * 1024)
	            if not data:
	                break
	            yield data

	response = Response(send_file(), content_type='application/octet-stream')
	response.headers["Content-disposition"] = 'attachment; filename=%s.zip' % dataset.title
	return response
```

## flask中model层运用@property装饰器
@property把方法属性化，这里把password方法看成属性，从而作为一个User表的字段。  
但是这个字段是无法直接读取的，直接读取会引发错误：No read property    
不过我们可以通过@password.setter来设置password字段

```
class User(db.Model):
    __tablename__ = "sys_user"
    id = db.Column(db.Integer, primary_key=True)
    account = db.Column(db.String(64), nullable=False, unique=True)
    password_hash = db.Column(db.String(128), nullable=False)
    nickname = db.Column(db.String(64))

    def __str__(self):
        return self.account

    @property
    def password(self):
        raise AttributeError("No read property")

    @password.setter
    def password(self, val):
        self.password_hash = generate_password_hash(val)

```
详见：https://www.liaoxuefeng.com/wiki/897692888725344/923030547069856


## python 中all的用处
```
def register():
    req_dict = request.get_json()
    account = req_dict.get("account")
    password = req_dict.get("password")
    if not all([account, password]):
        return jsonify(code=RET.PARAMERR, data="Incomplete parameters")
```


## flask实现只允许一个用户登录
```
if token != access_token:
    return jsonify(code=RET.LOGINERR,
        data="The account is already logged in on another device")
```

## Flask如何处理前端发来的get请求带bool值？
[ 描述 ]
```
前端发送get请求：
    isSuccess: false

后端接收请求：
value = request.args.get('isSuccess')
if value:
    print('yes')
else:
    print('no')

结果： 'yes'
```

[ 分析 ]
```
后端接收到 isSuccess = 'false'，其实是一个字符串，导致value永远都是True
```
[ 解决方案 ]
```
from flask_restful import inputs
value = request.args.get('isSuccess', False, type=inputs.boolean)
```

## 为什么flask任意层级的包都可以从顶部文件进行import ?
#### [ 解决 ]
因为程序入口在最顶层，顶层以下的包都可以随意导入。
有如下目录结构：
```
top
├── app.py
└── app                     // import app.layer_1.file_1
    ├── __init__.py
    ├── layer_1
    │   ├── file_1.py       // from app.layer_2.file_2 import file_2  print(file_2)
    │   ├── __init__.py
    └── layer_2
        ├── file_2.py       // file_2 = 'this is file 2'
        ├── __init__.py
```
运行 app.py :
```
"this.is file 2"

说明layer_1下的文件成功导入同级目录下的资源
```
