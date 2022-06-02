### Linux

![image-20220403225926544](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220403225926544.png)



##### 下载 VM

1.下载地址:

* 官方

```
https://www.vmware.com/cn.html
```

* 其他地址

```
https://www.nocmd.com/windows/740.html
```

2.安装Centos

* 去BIOS里修改设置开启 “虚拟化设备支持（f2,f10）”

* 安装虚拟机软件

```
1.在BIOS开启CPU虚拟化支持
https://jingyan.baidu.com/article/ab0b56305f2882c15afa7dda.html
```

* CentOS下载地址

```
- CentOS7.6下载：(https://mirrors.163.com/centos/)
http://mirrors.163.com/centos/7.6.1810/isos/x86_64/CentOS-7-X86_64-DVD-1810.iso

- CentOS8下载：（https://mirrors.aliyun.com/centos）
https://mirrors.aliyun.com/centos/8.1.1911/isos/x86_64
```

* Linux分区

```
1.boot分区（引导分区）
2.swap分区（交换分区）
3.根分区
```

* 网络连接三种方式

![image-20220405144107376](img\image-20220405144107376.png)

```
1.桥接模式:
- 虚拟系统可以和外部系统相互通讯，但是容易造成IP冲突
2.NAT模式:（生成虚拟网卡）
- 网络地址转换模式，虚拟系统可以和外部系统通讯，而且不造成IP冲突 
3.主机模式:
- 独立的系统
```

