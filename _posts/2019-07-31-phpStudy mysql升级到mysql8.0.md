---
layout:     post
title:      phpStudy mysql升级到mysql8.0
subtitle:   phpStudy mysql升级到mysql8.0
date:       2019-07-31
author:     BY
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - MYSQL
---
### 下载mysql8.0


[下载mysql8.0]: https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.12-winx64.zip
[下载mysql8.0]


### 编辑my.ini文件，下载的mysql8默认是没有my.ini的

```swift

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
 
[mysqld]
# 设置3306端口
port =3306
# 设置mysql的安装目录
basedir="D:/phpstudy/PHPTutorial/MySQL/"
#存放数据的目录
datadir="D:/phpstudy/PHPTutorial/MySQL/data/"
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8mb4
collation-server = utf8mb4_unicode_ci
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
#认证方式(如果这里不修改，会新版的密码认证，会连接不了数据库的，并且要放到mysqld下)
default_authentication_plugin = mysql_native_password

```

### 启动下 MySQL 数据库

管理员身份打开 cmd 命令行工具

- `D:\phpstudy\PHPTutorial\MySQL\bin>mysqld --initialize --console`


```swift
2019-07-31T06:16:27.277920Z 0 [System] [MY-013169] [Server] D:\phpstudy\PHPTutorial\MySQL\bin\mysqld.exe (mysqld 8.0.12) initializing of server in progress as process 18112
2019-07-31T06:16:35.635654Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 1+fY1S&)Nlky
2019-07-31T06:16:36.692361Z 0 [System] [MY-013170] [Server] D:\phpstudy\PHPTutorial\MySQL\bin\mysqld.exe (mysqld 8.0.12) initializing of server has completed
```

`A temporary password is generated for root@localhost: 1+fY1S&)Nlky` 后面密码

- `mysqld install`

```swift
Service successfully installed.
```

- `net start mysql`
```swift
MySQL 服务正在启动 .
MySQL 服务已经启动成功。
```

### 修改密码

- `mysql -uroot -p`进入mysql
- `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';`
- `quit;`
- `net stop mysql`关闭服务

### 切换到phpstydy客户度界面，重启服务
如果mysql启动失败

执行`D:\phpstudy\PHPTutorial\MySQL\bin>sc delete mysql`
```swift
[SC] DeleteService 成功
```
在重启一次就可以了