## 								docker生命周期指令 ##

`docker -h`查看命令

`docker info` docker详情

```
Client:
 Context:    default
 Debug Mode: false
 Plugins:
  app: Docker App (Docker Inc., v0.9.1-beta3)
  buildx: Docker Buildx (Docker Inc., v0.8.1-docker)
  scan: Docker Scan (Docker Inc., v0.17.0)

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 20.10.14
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 1
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc io.containerd.runc.v2 io.containerd.runtime.v1.linux
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 3df54a852345ae127d1fa3092b95168e4a88e2f8
 runc version: v1.0.3-0-gf46b6ba
```

`docker version` docker版本 

```
Client: Docker Engine - Community
 Version:           20.10.14
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 24 01:49:57 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.14
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.15
  Git commit:       87a90dc
  Built:            Thu Mar 24 01:48:24 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.11
  GitCommit:        3df54a852345ae127d1fa3092b95168e4a88e2f8
 runc:
  Version:          1.0.3
  GitCommit:        v1.0.3-0-gf46b6ba
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0

```

* run命令

`docker run ` 运行容器指令(运行完自动退出)

```
- 基本语法
# docker run [选项] IMAGE
- 基本选项
 -i:交互模式
 -t:分配一个终端
 -d:以后台的方式运行
 -name:指定容器的名称
# docker run --name mywebserver -itd nginx
```

* search命令

`docker search` 查找镜像

```
# docker search 镜像名称
```

* pull拉取镜像命令

`docker pull hello-word`下载镜像（从docker hub中下载）

```
# docker pull 镜像名
```

* docker启动nginx

```
# docker pull nginx
# docker run --name mywebserver -it nginx
- 报错信息
docker: Error response from daemon: Conflict. The container name "/mywebserver" is already in use by container "0fe5d922836dc9657a99758e6934fcbef04cfa9ac3d5b1788f9e9ee2881b4068". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'.
- 解决方案
1.更改名称 docker run --name mywebserver01 -itd nginx
2.删除容器 重新运行容器
```

* 查看运行容器IP地址信息命令

  `docker inspect 容器id`

* docker stats 显示容器所使用资源

  `docker stats 容器名/容器id`

