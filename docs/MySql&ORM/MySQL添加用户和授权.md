## 添加用户
```
本地登录：CREATE USER 'test'@'localhost' IDENTIFIED BY 'xxx';
远程登录：CREATE USER 'test'@'%' IDENTIFIED BY 'xxx';
```
## 授权
添加的新用户是没有任何权限的，需要授权
- 语法：
    ```sql
    GRANT privilege,[privilege],.. ON privilege_level 
    TO user [IDENTIFIED BY password]
    [REQUIRE tsl_option]
    [WITH [GRANT_OPTION | resource_option]];

    /**
    privilege: ALL|ALTER|CREATE...， 限定用户可以拥有的权限
    privilege_level: 全局(*.*)，数据库(database.*)，表(database.table)和列级别。 如果您使用列权限级别，则必须在每个权限之后使用逗号分隔列的列表。
    user: 放置要授予权限的用户。如果用户已经存在，则GRANT语句修改其特权。如不存在，则GRANT语句将创建一个新用户。可选的条件IDENTIFIED BY允许为用户设置新密码。

    之后，可指定用户是否必须通过安全连接(如SSL，X059等)连接到数据库服务器。最后，可选的WITH GRANT OPTION子句允许此用户授予其他用户或从其他用户删除您拥有的权限。此外，可以使用WITH子句来分配MySQL数据库服务器的资源，例如，设置用户每小时可以使用多少个连接或语句。这在MySQL共享托管等共享环境中非常有用。

    //原文出自【易百教程】，商业转载请联系作者获得授权，非商业请保留原文链接：https://www.yiibai.com/mysql/grant.html
    */
    ```

- 举例：为用户aaa授权使用数据库testDB，权限等级为ALL，并设置密码
    ```
    mysql> GRANT ALL ON testDB.* TO 'test'@'localhost' IDENTIFIED by '1234';
    ```

## 扩展
1. 查看所有用户
    ```
    SELECT user, host FROM mysql.user;
    ```
2. 修改用户密码
    ```
    方式1： mysql> set password for 'root'@'localhost' = password('123'); 

    方式2： 
    mysql> use mysql; 
    mysql> update user set password=password('123') where user='root' and host='localhost'; 
    mysql> flush privileges; 
    ```
3. 删除用户
    ```
    DROP USER 'jack'@'localhost';
    ```
4. 查看分配给用户的权限
    ```
    SHOW GRANTS FOR super@localhost;
    ```
