## 								Linux开发环境配置 ##

#### 一、概述

![image-20220519215313625](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220519215313625.png)

1.将jdk解压到/usr/local/java目录下

2.环境变量的配置文件在/etc/profile下

3.引入java的地址

4.引入java的地址到环境变量

```
# vim /etc/profile 在文件末尾加上配置:

export JAVA_HOME=/usr/local/java/jdk1.8.0_261

exprot PATH=$JAVA_HOME/bin:$PATH
```

5.source /etc/profile 让配置文件生效

### mysql 初始化

1.完成下载安装和环境变量配置

2.重命名mysql 和 赋权限

```
- 赋权限
# groupadd mysql
# useradd mysql -g mysql
# chown -R mysql:mysql mysql-8.0.29 //将文件夹权限给了mysql
# chmod -R 755 mysql-8.0.29 //添加rwx 4 2 1
参数:
-c显示更改的部分的信息

-f忽略错误信息

-h修复符号链接

-R处理指定目录以及其子目录下的所有文件

-v显示详细的处理信息

-deference作用于符号链接的指向，而不是链接文件本身
```

3.初始化命令

```
# ./mysql-5.7.22/bin/mysqld --user=mysql --basedir=/usr/local/mysql8/mysql-8.0.29 --datadir=/data/mysql-8.0.29/data/mysql --initialize
```

4.获取到mysql初始密码

```
!,5hrskkwbwR(随机生成)
```

5.修改mysql 配置文件

```
在mysql-8.0.29目录下:
cp support-files/mysql.server /etc/init.d/mysqld //将mysqld加入到service服务配置下

```

```
vim /etc/init.d/mysqld  //修改mysql服务配置文件
basedir：数据库安装位置
bindir： mysql的bin路径
datadir: 数据库文件存放位置
sbindir libexecdir
```

6.修改my.cnf文件 (很多参数如若启动失败,去error.log查看并处理)

vim /etc/my.cnf

```
[client]
no-beep
socket =/yearns/mysql-5.7.22/mysql/mysql.sock
# pipe
# socket=0.0
port=3306
[mysql]
default-character-set=utf8
[mysqld]
basedir=/yearns/mysql/mysql-5.7.22
datadir=/mysql-5.7.22/data
port=3306
pid-file=/yearns/mysql/mysql-5.7.22/mysqld.pid
#skip-grant-tables
skip-name-resolve
socket = /yearns/mysql/mysql-5.7.22/mysql.sock
character-set-server=utf8
default-storage-engine=INNODB
explicit_defaults_for_timestamp = true
#不区分大小写
lower_case_table_names=1
# Server Id.
server-id=1
max_connections=2000
query_cache_size=0
table_open_cache=2000
tmp_table_size=246M
thread_cache_size=300 
innodb_buffer_pool_size=256M
innodb_log_file_size=128M
innodb_thread_concurrency=128
innodb_autoextend_increment=1000
innodb_buffer_pool_instances=8
innodb_concurrency_tickets=5000
innodb_old_blocks_time=1000
innodb_open_files=300
innodb_stats_on_metadata=0
innodb_file_per_table=1
innodb_checksum_algorithm=0
back_log=80
flush_time=0
join_buffer_size=128M
max_allowed_packet=1024M
max_connect_errors=2000
open_files_limit=4161
query_cache_type=0
sort_buffer_size=32M
table_definition_cache=1400
binlog_row_event_max_size=8K
sync_master_info=10000
sync_relay_log=10000
sync_relay_log_info=10000
#批量插入数据缓存大小，可以有效提高插入效率，默认为8M
bulk_insert_buffer_size = 64M
interactive_timeout = 120
wait_timeout = 120
log-bin-trust-function-creators=1
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
#
# include all files from the config directory
!includedir /etc/my.cnf.d

```

7.启动mysq

```
./mysql_safe --user=mysql
```

8.mysql account&password

```
u:Admin
p:123
```

9.创建数据库

```
CREATE DATABASE 数据库名 [[DEFAULT] CHARACTER SET 字符集名] [[DEFAULT] COLLATE 校对规则名
```

