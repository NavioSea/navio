## 						Linux IDEA 2020安装 ##

#### 一、下载地址

https://www.jetbrains.com/idea/download/#section=linux

1.解压缩到/opt/idea目录下

2.启动idea bin目录下的 idea.sh，配置jdk

3.编写程序测试

## 								安装mysql5.7 ##

1.新建文件夹/opt/mysql

`mkdir /opt/mysql`

2.运行下载地址

wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
wget https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.22-linux-glibc2.12-x86_64.tar.xz

3.解压

`tar -vxf mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz`

*！注意:CentOs7自带的mysql数据库是mariadb，会跟mysql冲突,要先删除*

```
rpm -qa | grep mariadb //查询mariadb
rpm -e --nodeps mariadb-libs//删除相关
```

4.运行rpm -e --nodeps mariadb-libs,卸载

5.开始安装mysql5.7,运行下面指令

```
rpm -ivh mysql-community-common-5.7.26-1.el7.x86_64.rpm //安装通用common相关库
rpm -ivh mysql-community-libs-5.7.26-1.el7.x86_64.rpm //安装需要的libs
rpm -ivh mysql-community-client-5.7.26-1.el7.x86_64.rpm //安装mysql的client
rpm -ivh mysql-community-server-5.7.26-1.el7.x86_64.rpm //安装mysql的server
```

6.运行 systemctl start mysqld.service,启动mysql

7.设置root用户密码

mysql会随机生成一个root密码,运行 grep "password"/var/log/mysqld.log 可看到当前密码

8.进入mysql

​	`mysql -uroot -pxxxx` //登录mysql

9.设置密码

set global validate_pasword_policy=0 ；//设置提示密码设置策略(0 低1 中 2 高)

10. set password for 'root'@'localhost'=password("密码")
11. 运行flush privileges,刷新内存使密码设置生效

​	