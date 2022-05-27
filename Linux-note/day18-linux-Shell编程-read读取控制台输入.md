## 							read读取控制台输入

* 基本语法

  read [选项] [参数]

  选项:

  `-p` :指定读取值的提示符

  `-t`:指定读取值时等待的时间,如果没有在指定时间输入,就不等待了

* 参数

  变量:指定读取值的变量名

* 应用实例

  案例1: 读取控制台输入一个num值

  ```
  #!/bin/bash
  read -p "请输入一个数num1=" NUM1
  echo "NUM1=$NUM1"
  ```

  案例2:读取控制台输入一个num值,在10s内输入

  ```
  #!/bin/bash
  #10秒内输入一个数
  read -t 10 -p "请输入一个数num2=" NUM2
  echo "NUM2=$NUM2"
  ```

  ## 									系统函数 ##

* 系统函数 basename、dirname

  ```
  - basename 基本语法
  功能:返回完整路径/的部分，常用于获取文件名
  # basename [pathname] [suffix]
  # basename [string] [suffix]
  功能描述:basename会删掉所有的前缀包括最后一个('/')字符,然后将字符串显示出来。
  选项:
  suffix为后缀,如果suffix被指定了,basename将会pathname或string中的suffix去掉
  - 应用实例
  请返回/home/aaa/test.txt的"test.txt"部分
  # basename /home/aaa/test.txt .txt
  - dirname 基本语法
  功能:返回完整路径最后/的前面的部分,常用于返回路径的部分
  # dirname 文件的绝对路径 (功能描述:从给定的包含绝对路径的文件名中取出文件名，返回剩下目录部分)
  - 应用实例
  请返回/home/aaa/test.txt的"/home/aaa"部分
  # dirname /home/aaa/test.txt
  ```

  ## 									自定义函数 ##

* 基本语法

  ```
   function funname()
  {
  		Action;
  		return int;
  }
  调用直接写函数名: funname [值]
  ```

* 应用实例

  计算输入两个参数的和(动态获取),getSum

  ```
  #!/bin/bash
  #定义函数
  function getSum(){
  	SUM=$[$1+$2]
  	echo "和是=$SUM"
  }
  #输入两个值
  read -p "输入一个值 n1=" n1
  read -p "输入另一个值 n2=" n2
  #调用函数
  getSum $n1 $n2
  ```

  