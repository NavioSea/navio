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

