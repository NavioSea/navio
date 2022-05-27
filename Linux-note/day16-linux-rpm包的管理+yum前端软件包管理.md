## 									rpm包的管理 ##

### 注意！rpm包在DVD中的Packages目录下

#### 一、基本介绍

​		rpm用于互联网下载包的打包及安装工具,它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。RPM是RedHat Package Manager(Redhat软件包管理工具的缩写),类似windows的setup.exe,这一文件格式名称虽然打上了RedHat的标志,但是理念是通用的。

##### 

二、rpm包的管理

- *rpm包的简单查询指令*

  1.查询已安装的rpm列表 ` rpm -qa| grep xxx`

  案例:查看当前系统是否安装火狐 `rpm -qa | grep firefox`

![image-20220519142213883](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220519142213883.png)

* *rpm包的其他查询指令*

  1.查询所安装的rpm软件包：`rpm -qa`

  2.查询软件包是否安装：`rpm -q 软件包名` 例如:`rpm -q firefox`查看火狐是否安装

  3.查询软件包的信息: `rpm -qi 软件包名`  例如:`rpm -qi firefox`

  4.查询软件包中的文件地址: `rpm -ql 软件包名` 例如`rpm -ql firefox`

  查询文件所在的目录

  5.`rpm -qf 文件全路径名 查询文件所属的软件包`

  查询文件归属的软件包

  例如: `rpm -qf /etc/passwd`

   		 `rpm -qf /root/install.log`

* *rpm包的卸载*

  ```
  - 基本语法
  # rpm -e RPM包名     //erase（擦除）
  - 应用案例
  删除firefox 软件包
  # rpm -e firefox
  - 细节讨论
  1.如果其他软件包依赖于要卸载的软件包,卸载时会产生错误
  如:# rpm -e foo
  如果要 删除foo这个rpm包,可以增加参数 --nodeps 就可以强制删除(一般不推荐)
  # rpm -e --nodeps rpm包名
  ```

* 安装rpm包

  ```
  - 基本语法
  # rpm -ivh RPM包全路径名称
  参数说明:
  i=install 安装
  v=verbose 提示
  h=hash 	  进度条
  - 应用实例
  演示卸载和安装火狐
  # rpm -e firefox  //卸载
  # rpm -ivh firefox //安装
  ```

  ## 							yum前端软件包管理 ##

#### 一、基本介绍

* yum是一个Shell前端软件包管理器.基于RPM包管理,能够从指定的服务器自动下载RPM包并且安装,可以自动处理依赖关系,并且一次安装所有依赖的软件包。

* *yum的基本指令*

  查询yum服务器是否有需要安装的软件

  命令： `yum list |grep xx软件列表`

* *安装指定的yum包*

  命令：`yum install xxx`    //下载安装

* *yum应用实例*

  yum list installed xxx 

  *# 查看 yum 已安装 nginx的包* 

  yum list installed nginx 

  *# 查看 yum 已安装 nginx相关的包*

   yum list installed nginx*
  
* yum list installed

* yum remove 



