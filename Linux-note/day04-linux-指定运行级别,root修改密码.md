### 指定运行级别

* 运行级别说明

0：关机

1：单用户 【找回丢失密码】

2：多用户状态没有网络服务 

3：多用户状态有网络服务 

4：系统未使用保留给用户

5：图形界面

6：系统重启

命令:# init 【0123456】

* CentOS7后运行级别说明	

在centOS7以前,/etc/inittab文件中进行了简化，如下:

```
multi-user.target:analogous to runlevel 3 //多用户无界面
graphical.target:analogous to runlevel 5 //有用户界面
- 查看当前用户级别
# systemctl get-default
- 设置默认的运行级别
# systemctl set-default multi-user.target
```

### 找回root密码

#### 一、CentOS7.6之后版本

1.在开机界面处按	`e` 进入编辑界面

![image-20220427124402783](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220427124402783.png)

2.进入编辑界面，找到义"Linux16" 开头内容所在的行数,在该行最后输入:init=/bin/sh

![image-20220427124819746](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220427124819746.png)

3.填完后输入`ctrl+x`进入单用户模式，输入`mount -o remount,rw /`

![image-20220427125039920](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220427125039920.png)

4.输入两次密码

5.密码修改后，输入 `touch /.autorelabel`,回车

6.输入`exec /sbin/init` 等待系统自动重启,新密码生效