## nodejs连接mysql数据库[^报错Client does not support authentication protocol requested by server的解决方法]

MySQL8.0版本的加密方式和MySQL5.0的不一样，连接会报错。 解决方法如下：

```mysql
mysql> mysql -uroot -p; #登陆数据库
mysql> Enter password: ****** #输入root的密码 
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; #更改加密方式
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456'; #更改密码
mysql> FLUSH PRIVILEGES; #刷新
```
