## 									Dockerfile实践 ##

#### 1.docker build(构建镜像) 

```
当前目录:docker build -t image:v1 .
指定目录:docker build -t image:v1 -f 目录 .
不使用缓存--no-cache
```

1.1格式

```
## 指令 参数
## 指令不区分大小写,按照顺序执行Dockerfile
FROM 指定镜像
RUN 执行linux指令
#定义环境变量
ENV 
ADD
COPU
LABEL
```

2.0 定义环境变量ENV

```
ENV 名称 值
ENV 名称1=值1 名称=值
RUN echo > ${名称1}
```

2.1 RUN 执行指令

```
- shell方式执行
RUN mkdir -p /blade/system;指令2 
- exec格式执行
RUN ["executable","param1","param2"]
```

2.2参数ARG

```
ARG CODE_VERSION=7
FROM centos:${CODE_VERSION}
```

2.3 CMD指令(容器运行时需要提供的参数)

```
- exec格式
- shell格式
- ENTRYPOINT格式
```

2.4 LABLE标签 

```
LABLE key=value key=value
```

2.5 EXPOSE暴露容器端口

```
EXPOSE 8086/t 
```

 2.6 添加ADD (源文件放到目标文件)

```
ADD [--chown=<user>:<group><>] <src>... <dest>
ADD [--chown=<user>:<group><>] [<src>,... <dest>]
ADD 网络地址 目录
ADD 当前目录地址 目标目录地址
```

2.7 拷贝COPY

```
COPY [--chown=<user>:<group><>] <src>... <dest>
COPY [--chown=<user>:<group><>] [<src>,... <dest>]
COPY 当前目录地址 目标目录地址
```

2.8 入口点ENTRYPOINT

使用方法和CMD相同（exec和shell）

指定 ENTRYPOINT 指令为 shell 模式时，会**完全忽略命令行参数**

• 如果 ENTRYPOINT 使用了 shell 模式，CMD 指令会被忽略。
• 如果 ENTRYPOINT 使用了 exec 模式，CMD 指定的内容被追加为 ENTRYPOINT 指定命令的参数。
• 如果 ENTRYPOINT 使用了 exec 模式，CMD 也应该使用 exec 模式。

2.9 VOLUME [ " data" ]

```
VOLUME 挂载点
VOLUME ["挂载点"]
```

2.10 WORKDIR 工作目录

```
WORKDIR /path/to/workdir
```

yum install -y openssh-server

![image-20220530234226496](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220530234226496.png)

#### 构建镜像

docker build -t "镜像名:tagname" 镜像所在目录

#### 制作Centos基础镜像

1.安装sshd

`yum install openssh-server`

2.下载后将容器制作成镜像

`docker commit 容器 镜像名:TAGNAME`

#### 容器内服务以前台方式运行:(经典) ####

sshd前台启动命令:/usr/sbin/sshd -D

3.启动容器

--privileged=true :特权启动

```
docker run -itd --name="centos7_uat" -v /newdisk/app/centos7:/opt/data -p 2222:22 --privileged=true sshd_centos:v1 /usr/sbin/init
```

