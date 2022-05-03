# 					 压缩和解压缩类 #

#### `gzip`和`gunzip`指令（压缩文件和解压文件指令）

```
- 基本语法
# gzip 文件
# gunzip 文件.gz  
```

***

#### `zip`和`unzip`指令

```
# zip [选项] xxx.zip (功能描述:压缩文件和目录的命令)
# unzip [选项] xxx.zip (功能描述:解压缩文件)
- zip 常用选项:
 *选项1: -r  (递归压缩，即压缩目录)
- unzip 常用选项:
 * -d<目录>: 指定解压后文件的存放目录
- 应用实例:
 案例1: 将/home下的所有文件压缩成myhome.zip
# zip -r myhome.zip /home/
 案例2:将myhome.zip解压到/opt/tmp目录下
# unzip -d myhome.zip  /opt/tmp/myhome.zip
```

#### `tar`指令：是打包指令，最后打包后的文件是.tar.gz的文件

```
- 基本语法
# tar [选项] XXX.tar.gz 打包的内容 
（功能描述:打包目录，压缩后的文件格式.tar.gz）
- 选项说明
 *选项1: -c  (功能：产生.tar打包文件)
 *选项2: -v  (功能：显示详细信息)
 *选项3: -f  (功能：指定压缩后的文件名)
 *选项4: -z  (功能：打包时候压缩)
 *选项5: -x  (功能：解压.tar文件)
- 应用实例 
 案例1: 压缩多个文件,将/home/info.txt 和/home/detail.txt 压缩成pc.tar.gz\
# tar -zcvf /home/pc.tar.gz /home/detail.txt /home/info.txt 
* tar -zcvf targetFile resourceFiel1 resourceFile2
 案例2:解压到当前目录
# tar -zvxf tar.gz压缩文件 
 案例3:指定解压 
# tar -zxvf tar.gz压缩文件 -C 目标目录
```

