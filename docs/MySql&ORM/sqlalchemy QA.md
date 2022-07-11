---
title: sqlalchemy QA
date: 2021-10-31 13:37:18
permalink: /pages/b7a4b6/
categories:
  - MySql&ORM
tags:
  - 
---
## flask-sqlalchemy - 使用db.create_all()后有了数据库但是没有表

可能的情况1：  
```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_cors import CORS

app = Flask(__name__)
CORS(app)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///database/mydata.db'
db = SQLAlchemy(app)
db.init_app(app)    #### 问题在这里：需要把这一句加上去！！！

@app.route('/')
def hello_world():
    return 'Hello, World!'
```
可能的情况2：
```
~$ export 
~$ flask shell
>>>> from app import db
>>>> from models import MyData,...      #### 这一句很重要，把所有表都import进来，不然没有表
>>>> db.create_all()
```

可能的情况3：  
需要从model处导入db
```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///db.db'
db = SQLAlchemy(app)

@app.route('/')
def hello():
    return 'hello'

@app.cli.command('create_db')
def create_db():
    # from model import User
    from model import db       #### 重要
    db.create_all()

if __name__ == '__main__':
    app.run(debug=True)
```

## flask-sqlalchemy如何处理大量数据分页后，前端先要全选所有的数据并提交给后端处理。
```
了解到目前主流做法是选择的时候提示要不要全选，然后全选第一页，后端再进行处理。参考Gmail
```


## 链接数据库没权限
[ 描述 ]
```
mysql+pymysql:///xyz:xxxx@xxx:3306/xxx

=====>

sqlalchemy.exc.OperationalError: (pymysql.err.OperationalError) (1045, u"Access denied for user 'xyz'@'localhost' (using password: NO)")
```
[ 解决 ]
```
mysql+pymysql:///xyz:...  (错误)
mysql+pymysql://xyz:...   (正确)
```

## insert数据，只有commit之后才能拿到id

## scoped_session在多线程下会有死锁出现

## session.commit 和 session.flush区别
1. flush
```
如果只调用flush，那么更新虽然可以被写入数据库，但是事务是不完整的，没有提交。
```
2. commit
```
commit会先调用flush，再把更改提交到数据库，同时标志事务结束。
```
总结：
```
1. flush会生成primary key
2. 当前紧接着的session可以查到flush做的增删改的结果
3. 其他session只有在commit之后，才能查到flush做的增删改结果

ref：https://blog.csdn.net/weixin_38888144/article/details/106076056
```


## mysql如何并发写入？
[ 描述 ]
```
两个session，同时写入数据库同一个位置，会导致死锁，如何解决？
```

[ 错误思路 ]
1. 如果两个session都是写入，如下代码仍然会导致死锁。  
    ```py
    batch_data = session.query(Batch).with_for_update().filter_by(server_id=10).first()  # 也需要加锁

    if not batch_data:
        batch_data = Batch(server_id=10)
        session.add(batch_data)
        session.commit()  # !!标志事务结束，其他线程可以查询或者修改Batch表了。

    cycle_data = session.query(Cycle).with_for_update().filter_by(batch_id=batch_data.id)

    if not cycle_data:
        cycle_data = Cycle(batch_id=22)
        session.add(cycle_data)
        # session.commit()  # !!这里不能commit，事务不能在这里结束，因为后面我们还会更改cycle_data，如果需要用到新增的cycle id，则需要用flush
        session.flush()

    cycle_data.start_time = 999
    session.add(cycle_data)
    session.commit()  # !!在这里comimt，保证在这之前其它线程不会对cycle表（列）进行操作
    ```
2. 原因分析
    session1 | session2
    ---------|---------
    begin; | begin;
    select...for update | select...for update
    insert into …（等待） | insert into … （产生死锁）
