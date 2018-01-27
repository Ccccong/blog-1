# MySQL服务恢复手册

## Can't connect to MySQL server on localhost(10061)的解决办法

1、首先找到MySQL的安装目录，比如：`D:\MySQL Server5.1\bin`

2、在cmd中切换至该目录，然后执行命令：`mysqld --install`，输出`Service successfully installed`,则表明执行成功。

3、然后执行命令：`net start mysql`,如果服务正常启动则无需执行第4步。

4、第3步执行失败，则执行`mysqld --initialize --user=root --console`,在输出的内容中找到初始化后的随机密码记下来。
此时再次执行第3步.

5、使用初始化后的密码登录数据库，
    
    use mysql;
    update user set password=PASSWORD('新密码') where user='root';
   
   
## Access denied for user 'root'@'localhost'的解决方法

1、首先找到MySQL的安装目录，比如：`D:\MySQL Server5.1\bin`

2、在`D:\MySQL Server5.1`目录下找到my.ini文件，在文件末尾添加 `skip-grant-tables`，然后重启MySQL服务。

3、使用`mysql -uroot -p`免密登陆数据库

4、`use mysql`使用`mysql`数据库。

5、执行

```shell
    update user set password=PASSWORD('新密码') where user='root';
    
    flush privileges;
    
```

6、删除my.ini文件中最后一行添加的内容，重启MySQL服务。

此时就可以正常使用MySQL数据库了。