
## windows 命令
```
net start mysql
net stop mysql
```
## linux 命令
```text
service mysql status
# 启动mysql service
sudo service mysql start 
mysql -u root -p;
show databases;

create database trader charset=utf8;
```

## mysql 安装(linux)
通过apt 安装MySQL服务（会安装最新版）
log:
```text
done!
update-alternatives: 使用 /var/lib/mecab/dic/ipadic-utf8 来在自动模式中提供 /var
/lib/mecab/dic/debian (mecab-dictionary)
正在设置 mysql-server-8.0 (8.0.27-0ubuntu0.21.10.1) ...
update-alternatives: 使用 /etc/mysql/mysql.cnf 来在自动模式中提供 /etc/mysql/my.
cnf (my.cnf)
Renaming removed key_buffer and myisam-recover options (if present)
mysqld will log errors to /var/log/mysql/error.log
mysqld is running as pid 42590
Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service → /lib
/systemd/system/mysql.service.
```

```text
# 更新源
sudo apt update
# 安装mysql服务
sudo apt install mysql-server

安装验证插件
sudo mysql_sercure_installation

查看数据库状态
systemctl status mysql

编辑 非必需:注释掉bind-address = 127.0.0.1
/etc/mysql/mysql.conf.d/mysqld.cnf

创建新用户
create user '用户名'@'访问主机' identified by '密码';
CREATE USER 'admin'@'localhost' IDENTIFIED BY '你要设置的密码'; 
对新增的用户更改加密方式和密码:
ALTER USER 'admin'@'localhost' IDENTIFIED WITH mysql_native_password BY 'admin';
```

#### 配置文件

修改密码：
这个方法测试不行：
```
cat /etc/mysql/debian.cnf

mysql -u debian-sys-maint -p

然后就是修改密码了:
连接到mysql数据库
1)、use mysql;

2)、
8.0这个不行：
update mysql.user set authentication_string=password('xxxx') where user='root' and Host ='localhost';

用这个命令：
ALTER USER 'root'@'localhost' IDENTIFIED BY 'xxx';
update user set  plugin="mysql_native_password";

3)、update user set  plugin="mysql_native_password";     
4)、flush privileges;
5)、quit; 

最后测试：
sudo service mysql restart
```

这个修改密码并开启远程链接可以：
```
1.mysql -u root -p
2.enter 进入mysql
3.ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'xxxxx';

或则：
1.输入mysql进入服务mysql: mysql
2.如果进入不了，可能是mysql服务没有开，开启mysql
service mysql status
service mysql start
3.进去mysql之后先设置root密码，因为8.0版本已经没有passwd函数了:
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
4.FLUSH PRIVILEGES;

5.添加远程访问使用root访问数据库的权限并刷新权限
update user set host='%' where user='root';
FLUSH PRIVILEGES;

6.修改mysql配置文件
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
在里面把bind-address =127.0.0.1注释掉

7.防火墙打开3306端口并刷新防火墙设置
sudo ufw allow 3306
sudo ufw reload
sudo ufw enable
```

### 解决 show databases; 出错
```
在安装完 MySQL 后，使用 SHOW TABLES 时，发现一直出现 ERROR 1449 (HY000):The user specified as a definer (‘mysql.infoschema‘@’localhost’) does not exist 。具体去网上搜索，给的答案都是用 mysql_upgrade ，事实上 mysql_upgrade 功能在 mysql8.0 之后版本已经停用了。在经过多方查找后，找到如下解决方案。

总体办法就是给 mysql.infoschema 用户添加权限。
MySQL8.0 之后，不支持使用 grant 时隐式地创建用户，必须先创建用户，再授权。代码如下：

create user 'mysql.infoschema'@'%' identified by 'Abchen_123';
grant all privileges on *.* to 'mysql.infoschema'@'%';
flush privileges;
```