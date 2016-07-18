---
title: 在 windows 10 上安装 解压缩版 MySql
year: 2016
month: 07
day: 19
---


#### 下载 解压
Mysql  ：版本 5.7.13   [下载链接 ](http://120.52.73.10/cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.13-winx64.zip) （通过这个官网zip链接可以直接下载，不用再注册 Oracle 账号了）
解压 mysql-5.7.13-winx64.zip 到 任意目录，我解压到了 “D:/Program Files/mysql/mysql-5.7.13-winx64” 
#### 配置系统变量
在系统变量 PATH 中加一条 “D:\Program Files\mysql\mysql-5.7.13-winx64\bin”
#### 修改默认配置文件
在目录 “mysql-5.7.13-winx64” 下有配置文件 my-default.ini ，复制一份为 my.ini。
修改 my.ini 中的 basedir 和 datadir
```powershell
basedir =  "D:/Program Files/mysql/mysql-5.7.13-winx64"

# 执行mysqld  --initialize命令后会初始化这个目录
datadir = "D:/Program Files/mysql/mysql-5.7.13-winx64/data"

# server_id 也一定要设，不然会报错 [ERROR] You have enabled the binary log, but you haven't provided the mandatory server-id.Please refer to the proper server start-up parameters documentation
# 2016-07-18T15:29:23.097024Z 0 [ERROR] Aborting
server_id = 1 
```


#### 安装
在  mysql-5.7.13-winx64\bin  目录下执行  `mysqld  --initialize --console`，一定要是 bin 目录,并且一定要有 --console ，最后一行可以看到生成的密码，看不到密码可不就不能登录了么。
```shell
PS D:\Program Files\mysql\mysql-5.7.13-winx64\bin> mysqld  --initialize --console
2016-07-18T15:56:17.820761Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defau
lts_for_timestamp server option (see documentation for more details).
...
...
...
2016-07-18T15:56:18.449148Z 1 [Note] A temporary password is generated for  root@localhost: :esIkHCaG2,d 
```

这条命令会在 my.ini 设定的 datadir 目录下初始化生成很多文件。如图 
 ![mysql_data](/images/mysql_data.png)



#### 启动

然后安装 mysql   `mysqld install`   重装 mysql 时需要 卸载 mysql  ，命令 `mysqld remove`

然后启动 mysql 

```powershell
PS D:\Program Files\mysql\mysql-5.7.13-winx64\bin> net start mysql
MySQL 服务正在启动 .
MySQL 服务已经启动成功。
```



运行命令 `bin\mysql_secure_installation.exe` 进行 安全化设置，比如 重置密码，是否允许远程访问。



启动 mysql后 运行 `mysql -u root -p`  即可进行数据库操作了，比如用以下命令看看。

```
# 显示有哪些数据库
show databases;
# 进入数据库
use test;
# 显示表
show tables;
```

