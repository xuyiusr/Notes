# Mysql 配置

## Mysql官方文档

> https://dev.mysql.com/doc/

## 配置

查看初始密码

```shell
#yum安装
cat /root/.mysql_secret
#rpm安装
grep 'temporary password' /var/log/mysqld.log
```

更换密码

```mysql
# method 1
mysql> SET PASSWORD = PASSWORD('Ankki_mySQL123');
# method 2
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'Ankki_mySQL123';
# method 3
shell> /usr/bin/mysqladmin -u root password 'Ankki_mySQL123'
# method 4
mysql> UPDATE user SET password=PASSWORD("Ankki_mySQL123") WHERE user='root';
```

修改为免密登录

```shell
vim /etc/my.cnf
#添加
[mysqld]
skip-grant-tables=1
#重启mysql
```

添加远程连接权限

```mysql
#添加授权
use mysql;
grant all privileges  on *.* to root@'%' identified by "Ankki_mySQL123" ;
#查看授权情况
select host,user,password from user;
flush privileges;
```

修改密码为永不过期

```mysql
use mysql;
update user set password_expired='N' where user='root';
flush privileges;
```

修改数据库安装目录，如/data目录

```shell
vim /etc/my.cnf
#修改
[mysqld]
datadir  = /data/mysql
#停止mysql
#将原安装目录文件移动到新目录
mv ($old_path)/mysql /data/
#重启mysql
```

报错 (28000): Access denied for user 'root'@'127.0.0.1' (using password: YES)”

```shell
#1. 检查my.cnf的配置是否有以下参数skip-name-resolve#跳过域名解析。此时127.0.0.1和localhost对mysql数据库来讲，是不同的主机，而root@'127.0.0.1'为空密码

#2. 检测user表信息
#user中有多条root记录时，mysql会优先判断是否使用了绑定的ip，所以可以把user表中绑定固定Ip的记录删除，只保留host字段为localhost或%的记录。
```