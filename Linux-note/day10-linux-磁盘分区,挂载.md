## 							Linux磁盘分区、挂载 ##

#### 一、基本原理

* 磁盘分区和文件目录的关系

![image-20220514154224029](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220514154224029.png)

Linux是将磁盘分区挂载到某一个系统文件目录中,将一个分区华人一个目录联系起来。

* 查看所有设备挂载情况

  1.`lsblk`

  ![image-20220514155007741](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220514155007741.png)

  2.`lsblk -f`

#### 二、Linux硬盘说明

![image-20220514154442978](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220514154442978.png)

#### 三、挂载的经典案例

* 增加一块硬盘

  ```
  1.虚拟机添加硬盘
  2.分区
  3.格式化
  4.挂载
  5.设置可自动挂载
  ```

2.分区`将新增磁盘sdb挂载到dev设备目录上`

* 分区命令 `fdisk /dev/sdb` (dev目录是设备文件目录，所有的设备都挂载在dev目录下)

* 命令

  ![image-20220514160610241](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220514160610241.png)

```
m:显示命令列表
p:显示磁盘分区（等同 fdisk -l） 
n:新增分区
d:删除分区
w:写入并退出
q:不保存退出
```

3.格式化磁盘(`分区后未生成uuid`)

格式化磁盘命令 `mkfs -t ext4 /dev/sdb1` 其中`ext4`是分区类型

4.挂载：将一个分区和一个目录联系起来

​	命令: `mount 设备名称 挂载目录`

​	例如:`mount /dev/sdb1 /newdisk`

​	卸载:`umount 设备名称 挂载目录`

​	例如：`umount /dev/sdb1`  或者  `umount /newdisk`

5.永久挂载（自动生效）

​	永久挂载:通过修改`/etc/fstab`实现挂载

​	`vim /etc/fstab`

​	添加完成后 执行mount -a 即刻生效

