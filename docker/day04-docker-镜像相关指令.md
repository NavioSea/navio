## 								Docker镜像操作 ##

1.登录和注销镜像仓库login/logout Docker registry（拉取私有镜像需要验证）

```
# docker login
```

2.下载镜像 `docker pull 镜像名`

3.查找镜像 `docker search 镜像名`

4.重命名镜像 

``` 
# docker tag hello-world:latest navioSea/docker:latest
# docker tag 镜像名:latest DockerId/DockerHub仓库名:latest
```

5.上传镜像到镜像仓库

```
# docker push DockerId/Docker镜像仓库:tagname
```

6.基于容器提交一个新的镜像

```
# docker commit [选项]
 *-a 作者姓名
 *-m 提交信息
 *-c 变化列表
 *-p 暂停提交
# docker commit -a 作者信息 容器名 镜像名:版本
```

7.docker build 构建镜像

```
1.编写Dockerfile
Dockerfile
FROM 镜像:tag
# docker build .
2.执行命令
# docker build -t 镜像名:tagName
```

8. 导入镜像import命令

```
# docker import test.tar webserver:v1
```

