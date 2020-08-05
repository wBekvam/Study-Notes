# Linux连接mysql报错

#### 问题：Access denied for user ‘root’@‘localhost’(using password: YES)的解决方法最

近在linux连接mysql /usr/local/mysql/bin/mysql -uroot -p

输入密码出现Access denied for user 'root'@'localhost'(using password: YES)错误。下面话不多说了，来一起看看详细的解决方法吧



#### 解决办法：

##### 1.先停止mysql 服务

```
service mysqld stop
```

##### 2.进入mysql安装目录bin/ 使用safe模式，进行重启：

查看数据库安装目录：

​			

```
./mysqld_safe --skip-grant-tables
```

![image-20200311172647112](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200311172647112.png)

##### 3.使用root账户，无密码登录，修改root用户密码

```
mysql -u root 

use mysql

update user set password=PASSWORD("你的密码") where User = 'root';
```

/usr/local/mysql/bin/mysql -uroot -p 然后输入刚才重置的密码

