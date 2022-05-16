#### 一、磁盘使用情况                             

1.查看磁盘使用率

`df -h` 默认查询当前目录

`df -h /目录` 查询指定目录

```
- 基本使用
df [选项]
 -s: 指定目录占用大小汇总
 -a: 含文件
 --max-depth=1: 子目录深度
 -c:累出明细的同时，增加汇总值
- 应用案例
1.查询/opt目录，子目录深度为1的磁盘使用情况
# du -h --max-depth=1 /opt
```

![image-20220516161922833](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220516161922833.png)

2.磁盘情况-工作实用指令

```
1.统计/opt文件夹下文件的个数
# ls -l /opt | grep "^-" | wc -l
2.统计/opt文件夹下的目录个数
# ls -l /opt | grep "^d" | wc -l
3.统计/opt文件夹下的文件个数,包括子文件夹里的
# ls -lR /opt | grep "^-" | wc -l
4.统计/opt文件夹下的目录个数，包括子文件夹里的
# ls -lR /opt | grep "^d" | wc -l
5.以树状显示目录结构(如果没有tree指令,使用yum install tree 安装)
# tree /目录
```



