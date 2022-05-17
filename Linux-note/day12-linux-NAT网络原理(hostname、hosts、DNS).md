## 								 NAT网络原理  

#### 一、虚拟机网络通讯原理

![image-20220516212016261](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220516212016261.png)

**宿主机和服务器主机在同一个网段,网络在关闭防火墙后是互通的* 

1.局域网访问外网的网络图（NAT的网络配置）

![image-20220516213016918](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220516213016918.png) 

#### 二、Linux具体网络配置

1.自动获取

系统工具==>设置==>`IPV4`(自动)

2.指定`IP`

```
- 说明
 直接修改配置文件指定IP,并可以连接到外网
 编辑 vi /etc/sysconfig/network-scripts/ifcfg-ens33
 要求将IP地址配置的静态的，比如:ip地址为192.168.200.130
- ifcfg-ens33文件说明
 DEVICE=eth0      #接口名(设备、网卡)
 HWADDR=00:0C:2x:6x:0x:xx      #MAC地址
 TYPE=Ethernet    #网络类型(通常是Ethernet)
 UUID=XXXXXX-XXXXX-XXXXX-XXXX    #随机ID
 #系统启动的时候网络接口是否有效(yes/no)
 ONBOOT=yes
 #IP的配置方法[none|static|bootp|dhcp] (引导时不适用协议|静态分配IP|BOOTP协议|DHCP协议)
 BOOTPROTO=static
 #IP地址
 IPADDR=192.168.200.130
 #网关
 GATEWAY=192.168.200.2
 #域名解析器
 DNS1=192.168.200.2
```

3.重启网络服务

`service network restart`或者`reboot`

#### 三、设置主机名和Hosts映射(重点）

1.指令`hostname` : 查看主机名

​    修改主机名:`vim /etc/hostname`

3.设置hosts映射

   修改hosts映射: ` vim /etc/hosts`

#### 四、DNS

1.`DNS`(domain name system) 域名系统

2.是互联网上作为域名和`IP`地址相互映射的一个分布式数据库

 本地`DNS`缓存=>hosts文件=>本地域名服务器=>根域名服务器=>顶级域名服务器=>权威域名服务器![image-20220517184525574](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220517184525574.png)

