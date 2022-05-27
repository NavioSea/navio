## 						Docker容器和镜像指令 ##

#### 1.save（把一个或多个镜像转换为tar包）

```
# docker save -o 生成文件名.tar 镜像:tagname ...
```

2.加载镜像 load

```
# docker load -i 导出的镜像包
```

3.拷贝文件

```
scp 文件目录 账号@IP:文件目录
```

4.创建一个新的容器

```
# docker creat --name 容器 镜像
通过docker ps -a 查看 docker start 启动
```

#### 2.定义一个Dockerfile容器

1.编写Dockerfile

```
FROM 镜像:tagName #基础镜像
WORKDIR /newdisk/app  #指定工作目录
COPY package.json .
RUN npm install #进行安装
EXPOSE 8080  #端口
CMD [ "npm","start" ]  #运行npm start
COPY ..  #复制当前脚本
```

2.构建docker

`docker build -t 镜像:tagname 目录`

-t/-tag:重命名

 #### 3.创建一个容器

`docker run -itd bullent  -p 8585:8080 --name bullent-work`

#### 4.上传镜像仓库

1.修改本地镜像

`docker tag  bullent:1.0 DockerId/bullent:1.0` 

`docker push  naviosea/bullent:1.0`