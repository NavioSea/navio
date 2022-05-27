## 						docker容器相关其他指令 ##

1.暂停/开始（暂停状态）

```
# docker pause/unpause 容器名
```

2.容器重命名 rename

注意：查看某个指令使用方式 docker rename -h

```
# docker rename 旧的容器名 新的容器名
```

3.查看容器信息 inspect

```
# docker inspect 容器名 //显示容器信息
```

4.镜像更改历史记录

```
# docker history 镜像名
```

5.kill杀死进程(关闭容器进程)

```
# docker kill 容器名
```

6.查看容器进程

```
# docker top 容器  [ps 进程id]
```

7.进入容器(连接容器)

```
1.exec
# docker exec -it 容器名 bash 
2.attach
# docker attach webserver //将本地的错误输出给本地容器
```

8.把文件拷贝进进容器中,把容器中的文件拷贝出来

```
# docker cp /etc/hosts webserver:/tmp (两个路径调换可以实现相互拷贝)
```

9.查看docker发生改变的文件(文件系统记录被修改的文件)

```
# docker diff 容器名
```

10.查看容器事件 `docker events`

11.导出容器

```
# docker export -o  test.tar  容器名
# mkdir docker
# mv test.tar docker/
# cd docker/
# tar xf test.tar
```

