## 								Shell 编程运算符 ##

#### 一、基本介绍

在shell中进行各种运算操作

```
- 基本语法
1."$((运算式))" 或"$[运算式]" 或者 expr m + n//expression表达式
2.注意expr运算符间要有空格
3.expr m - n
4.expr \*,/,% 乘除取余
- 应用实例
案例1: 计算（2+3）*4 的值
VALUE=$(((2+3)*4))

VALUE=$[(2+3)*4]

RESULT1=`expr 2 + 3`
VALUE=`expr RESULT1 \* 4`
案例2:请求出命名行的两个参数[整数]的和
#!/bin/bash
echo "VALUE=$[$1+$2]"
```

## 										条件判断 ##

```
- 基本语法
[ condition ] (注意condition前后有空格)
#非空返回true,可使用$?验证(0为true,>1为false)
- 应用实例
[ 不为空 ] 返回true
[ ] 返回false
[ condition ] && echo OK || echo notok  条件满足,执行后面的语句
```

常用的判断语句

![image-20220520204615662](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220520204615662.png)

1）`=` 字符串比较

2）两个整数的比较

`-lt` , `-le` ,  `-eq`,  `-gt`,  `-ge` ,  `-ne`

3）按照文件的权进行判断

`-r`, `-w`, `-x`

4）按照文件类型进行判断

`-f`  文件存在并且是一个常规的文件

`-e`  文件存在

`-d`  文件存在并且是一个目录

```
- 应用实例
案例1:"ok"是否等于"ok"
#!/bin/bash
if [ $1 = $2 ]
then
	echo "equal"
else
	echo "not equal!"
fi 
* fi 代表结束
案例2: 23是否大于等于22
if [ $1 -ge $2 ]
then
	echo "大于"
else
	echo "不大于"
fi	
案例3: /newdisk/shcode/preVar.sh 是否存在
if [ -e /newdisk/shcode/$1 ]
then
	echo "存在"
else
	echo "不存在"
fi	
```





