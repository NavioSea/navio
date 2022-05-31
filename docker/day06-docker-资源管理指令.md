##   				docker数据卷

 #### 1.容器持久化

1.创建一个数据卷

`docker volume create --name myvolume`

![image-20220527165019939](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220527165019939.png)

！ 创建的数据一般存在/var/lib/docker/volumes/数据卷名/_data下面

2.查看数据卷列表

`docker volume ls`

3.查看数据卷详情

`docker volume inspect myvolume`

在挂载点查看数据

*4.在启动docker容器时,把它数据卷挂载到文件系统*

`docker run -itd --name 容器名 -v 挂载目录:/data:rw 镜像名:tagname`

#### 2.共享Volume

方式一:容器共享数据卷的挂载目录

 docker run -itd --name 容器名 -v 挂载目录:/data:rw 镜像名:tagname

方式二: -volumes-from

docker run -itd --name 容器名 --volumes-from 共享容器名 镜像名:tagname

#### 3.删除volume

1.通过`docker volume rm 数据卷名`直接删除

2.通过--rm删除(当容器停止后会删除容器,数据卷不变化)

docker run -itd --rm --name 容器名 -v 挂载目录:/data:rw 镜像名:tagname

