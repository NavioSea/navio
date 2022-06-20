## 							`DockerRegistry` ## 

1.启动容器

`docker run -d -p 5000:5000 --resstart=always --name registry -v /opt/registry:/var/lib/registry registry `

注意:没有镜像,回自动拉取镜像

重启docker服务，容器全部退出解决方法:

![image-20220531152357439](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220531152357439.png)

2.修改配置

`vim /etc/docker/daemon.json`

![image-20220531152902848](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220531152902848.png)

`docker tag nginx:latest ip:port/DockeId/镜像名:tagname`

```
{
"registry-mirrors": ["http://uoggbpok.mirror.aliyuncs.com"],
"insecure-registries":["192.168.200.130:5000"],
"graph":"/newdisk/app/docker"
}

```

#### Harbor镜像仓库

Harbor地址:https://goharbor.io/

Harbor下载地址:https://github.com/goharbor/harbor/releases/download/v2.5.1/harbor-offline-installer-v2.5.1.tgz

1.解压后执行`install.sh`脚本

2.修改harbor.yml文件

#### 安装docker-compose

1、到官网下载docker-compose的离线包

下载地址：https://github.com/docker/compose/releases

链接:https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64

下载离线:http://t.zoukankan.com/braveym-p-15633596.html

两种最新的docker安装方式

1.从github上下载docker-compose二进制文件安装
下载最新版的docker-compose文件 

```
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

 若是github访问太慢，可以用daocloud下载

```
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
```

添加可执行权限

```
sudo chmod +x /usr/local/bin/docker-compose
```

测试安装结果

```
$ docker-compose --version

docker-compose version 1.16.1, build 1719ceb
```

2.pip安装

```
sudo pip install docker-compose
```



