## 									服务管理

#### 一、基本介绍

​		服务(service)本质是进程,但是运行在后台,通常都会监听某个端口,等待其他程序的请求（如:`mysql`）,因此我们又称为守护进程。（*重要*）

#### 二、service管理指令

1.`service  服务名  [start | stop | restart | reload | status]`

2.`CentOS7.0后很多服务不再使用service,而是systemctl`

3.`service指令管理的服务在/etc/init.d目录下查看`

* 查看服务名 `setup`

![image-20220518133734421](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220518133734421.png)

* 服务运行级别

![image-20220518133940827](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220518133940827.png)

#### 三、`chkconfig指令`

````
- 基本介绍
1.通过chkconfig命令可以给服务的各个运行级别设置自 启动/关闭
2.chkconfig指令管理的服务在/etc/init.d 查看
- 基本语法
 * 查看服务 chkconfig --list | grep xxx //查看服务列表
 * chkconfig 服务名 --list  //查看服务的运行级别情况
 * chkconfig --level [运行级别] [服务名] on/off  //开启或关闭服务的某个运行级别
- 使用细节
chkconfig重新设置服务后自启动或关闭,需要重启机器reboot生效
````

#### 四、systemctl指令（重点）

* systemctl 指令

1.基本语法

 `systemctl [start | stop | restart | status] 服务名`

2.systemctl指令管理的服务在 /usr/lib/systemd/system 查看

* systemctl设置服务自启动状态

1.systemctl list-unit-files [|grep 服务名] （查看服务开机启动的状态）

2.systemctl  enable 服务名 (设置开机自启动)

3.systemctl  disable 服务名 (关闭服务开机自启动)

4.systemctl is-enabled 服务名 (查询某个服务是否开机自启动)

* 应用实例

查看防火墙的状况

`chkconfig --list |grep`  firewalld.service//运行级别

`ls -l /usr/lib/systemd/sytem | grep firewalld.service` //查看防火墙服务

`systemctl list-unit-files | grep firewalld.service`//查看防火墙开机启动的状态 

关闭防火墙 `systemctl stop firewalld.service`

重启防火墙 `systemctl restart firewalld.service`

* 使用细节

1.关闭或者启动防火墙后,立即生效.[telnet 测试IP 某个端口]

`telnet 192.168.200.130 111` //访问111端口

2.`systemctl stop 服务名`这种方式是临时生效,重启系统后仍然回归之前的设置

解决：系统自启动systemctl [enable|disable] 服务名

3.如果希望对某个服务永久生效，要使用systemctl [enable | disable] 服务名

#### *查看网络状态的指令*：`netstat -anp | more`

```
netstat
-a : 将目前系统上所有的连接、监听、socket信息都列出来
-t ：列出tcp网络封包的信息
-u ：列出udp网络封包的信息
-n ：不以进程的服务名称，以端口号来显示
-l ：列出目前正在网络监听的服务
-p ：列出该网络服务的进程PID	

#常用的参数
netstat -anp | grep 端口号  //筛选出端口号
netstat -aultp  //查看哪些端口被占用
```

#### 五、防火墙端口

* #### 打开或关闭指定端口

  在真正的生产环境,往往需要将防火墙打开,如果把防火墙打开,外部请求数据即时通讯会被防火墙拦截。此时就需要打开指定的端口.

* firewall指令

  * 打开端口: firewall-cmd --permanent --add-port=端口号/协议
  * 关闭端口: firewall-cmd --permanent --remove-prot=端口号/协议
  * 重新载入,才能生效: firewall-cmd --reload
  * 查询端口是否开放: firewall-cmd --query-port=端口号/协议

* 应用案例

  1.启用防火墙,测试111端口是否能telnet

  `firewall-cmd --query-prot=111/tcp //查询111端口是否开放

  `telnet 192.168.200.130 111` //测试连接111端口

  2.开放111端口

  `firewall-cmd --permanent --add-port=111/tcp`

  `firewall-cmd --reload`//添加开放完毕需重新载入

  3.再次关闭111端口

  `firewall -cmd --permanent --remove-port=111/tcp`

  `firewall-cmd --reload`//关闭端口完成需重新载入