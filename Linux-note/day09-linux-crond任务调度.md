## 				crond任务调度（循环任务调度） ##

#### crontab进行 定时任务的设置

```
- 基本语法
# crontab [选项]
常用选项:
 * -e  编辑crontab定时任务
 * -l  查询crontab任务
 * -r  删除当前用户所有的crontab任务
```

* 快速入门

  ```
  1.找到配置调度文件
  设置任务调度文件:/etc/crontab
  2.编辑任务调度
  设置个人任务调度.执行crontab -e命令
  3.输入任务到调度文件
  */1 * * * * ls -l /etc/ > /tmp/to.txt命令
  每小时的每分钟执行ls -l /etc/ > tmp/to.txt命令
  ```

* 应用实例

  案例1:每隔1分钟，就将当前的日期信息，追加到/tmp/mydate文件中

   ` */1 * * * * date > /tmp/mydate` 

  案例2:每隔1分钟,将当前日期和日历都追加到/home/mycal文件中

  ` vim /home/mycal.sh` 编辑文件

  ```
  `*/1 * * * *  date >> /home/mycal.sh`
  `*/1 * * * * cal >> /home/mycal.sh`
  ```

  `chmod u+x  /home/mycal.sh`修改权限

  `crontab -e`编辑定时任务

  ```
  */1 * * * * /home/mycal.sh`//添加可执行的执行文件
  ```

  案例3:每天凌晨2点将mysql数据库testdb,备份到文件中。指令为mysqldump -u root -p密码 数据库 > /home/db.bak

#### crontab相关执行指令

* conrtab -r:终止任务调度

* conrtab -l:列出当前有哪些任务调度

* service crond restart [重启任务调度]

  ## 			at定时任务（一次性定时计划任务） ##

#### at定时任务

* 基本介绍

  1.at命令是一次性定时计划任务,at的守护进程会以后台模式运行,检查作业队列来运行

  2.默认情况下,atd守护进程每60s检查作业队列，有作业时，会检查作业运行时间,如果时间与当前时间匹配，则运行此作业

  3.at命令是一次性定时计划任务,执行完一个任务后不在执行此任务了

  4.在使用at命令的时候,一定要保证atd进程的启动,可以使用相关指令来查看

  查看全部进程指令 `ps -ef `

  查看atd任务队列进程 `ps -ef | grep 'atd'`

#### atd执行原理

![image-20220508185558963](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220508185558963.png)

#### at命令格式

* ctrl+d 结束at命令的输入

* at [选项] [时间]

  选项:![image-20220508185704851](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220508185704851.png)

  at时间定义:

  ```
  1.接受在当天的hh:mm(小时:分钟)式的时间指定.加入该时间已过去,那么就放在第二天执行。04:00
  2.使用midnight（深夜）、noon（中文）、teatime(下午4点)登比较模糊的词语来指定时间
  3.采用12小时计时制，即在时间后面加上AM或者PM.例如:12pm
  4.指定命令执行的具体日期,指定格式为moth day(月日)或mm/dd/yy(月/日/年)或dd.mm.yy(日.月.年)。例如:04:00 2021-03-1
  5.使用相对计时法,指定格式为:now + count time-units,now就是当前时间,time-units是时间单位,这里能够是minutes,hours,days,weeks。count是时间的数量。例如:now+5 minutes
  6.直接使用today\tomorrow来指定完成命令的时间
  ```

* 案例：

  1.明天2点执行一条命令

  * `at 2pm + 2 days` 执行时间

  * `cal > /tmp/mycal.log`  命令

  * Ctrl+D （2次）

  2.2分钟后执行某条脚本

  * `at now + 2 minutes`
  * /home/mydate.sh
  * Ctrl+D (2次)