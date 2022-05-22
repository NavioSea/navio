# Docker（www.docker.com）

![image-20220418223825324](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220418223825324.png)

- data volumes:（支持数据持久化）

* CI/CD: 持续集成 持续部署

![image-20220522143123005](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220522143123005.png)

1.下载镜像

```
mirrors.tuna.tsinghua.edu.cn
打开docker-ce //社区版
```

2.安装    

Install docker 1

```
cd /etc/
替换下载地址
# vim docker-ce.repo 
1.找到[docker-ce-edge]
2.替换baseurl=http://download.docker.com/linux/centos/7/$basearch/edge:
%s/download.docker.com/mirrors.tuna.tsinghua.edu.cn\/docker-ce/g
```

```
# yum clean all
下载
# yum -y install docker-ce
```

查找下载的包

Install docker 2

```
yum install epel-release
# 下载dnf
yum install dnf
#
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
# 查看docker-ce
dnf list docker-ce
```

```
dnf install docker-ce --nobest -y
# 启动docker
systemctl start  docker
# 自启动docker
systemctl enable docker
#查看docker版本
docker -v
```

图例:

![image-20220522154438745](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220522154438745.png)