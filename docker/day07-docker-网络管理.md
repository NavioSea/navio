## 								docker网络 ##

#### 1.docker默认的网络模式是桥接模式

![image-20220527203731663](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220527203731663.png)

#### 2.network的创建

`docker network  create mynet2`

`docker network ls`

容器启动指定

`docker run -itd --name centos01 --net mynet centos:7`

![image-20220527205514982](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220527205514982.png)

#### 

#### 3.路由设置

1.`route -n`或者`IP routing table`:查看路由表

2.添加路由

route add -net 192.168.200.130 netmask 255.255.0.0 dev eth0

3.删除路由

route del -net 192.168.200.130 netmask 255.255.0.0 dev eth0

 4.连接网络

`docker network connect mynet centos`