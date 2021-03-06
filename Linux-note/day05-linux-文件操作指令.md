#### 帮助指令

`man [指令]` ：查看该指令的使用功帮助信息详情和解释(获得帮助信息)

例如:` man ls`

`help [指令]  `:  获得shell内置命令的帮助信息

例如: `help cd`

#### 文件目录类

`pwd` :产看当前所在目录(绝对路径)`

`mkdir 【目录】`:用于创建目录

```
创建多级目录: mkdir -p 【】
```

`rmkdir 【目录】`: 删除空目录

`rm -rf 【目录】`：强制删除  

`touch 【文件名称】`: 创建文件目录

#### 拷贝指令

`cp -r 【resource】【/target/】`: 递归拷贝 

`\cp -r  【resource】【/target/】`:  拷贝默认覆盖

-f :强制

-r ：递归

rm -rf 【目录】: 强制删除不提示

#### 移动文件指令

```
1.同级目录下:重命名
# mv【old】 【new】
2.不同目录下:剪贴
# mv 【/dir1/】【/dir2/】
3.产看文件内容
# cat 【file】
 常用选项: -n 显示行号
 使用细节:cat [管道命令 | more]
# cat -n /etc/profile | more [进行交互]
```

####  more 指令：基于vi编辑器的文本过滤器，具体快捷键有:
* 空格：翻页 
* 回车：向下翻1行
* `q`：标识立刻离开more,不再显示该内容
* `ctrl+F`：向下滚动一屏
* `ctrl+B`：返回上一屏=：输出当前行号
* `:f`：输出文件名和当前行号

#### less指令：用来分屏查看文件的内容，功能类似于more指令

#### （根据显示需求部分加载，效率高）

* 具体操作快捷键：

```
- 空格: 向下翻动一页
- [pageUp]: 上翻页
- [pageDown]: 下翻页
- /字串: 向下搜索[字串]的功能；n:向下查找 N:向上查找
- ?字串: 向上搜索[字串]的功能；n:向下查找 N:向上查找
- q: 离开less程序
```

#### echo指令:输出内容到控制台

```
- 基本语法
# echo [选项] [输出内容]
案例: 输出环境变量,比如输出 $PATH
# echo $PATH
```

#### head指令：用于显示文件开头部分内容（默认前十行）

```
- 基本语法
# head -n [行数] 【文件】 
```

#### tail指令：用于输出文件尾部的内容，默认tail指令显示文件的尾十行内容

```
- 基本语法
# tail 文 件： (查看文件尾10行)
# tail -n 5 文件： （查看尾5行）
# tail -f 文件：（实时追踪该文档的所有更新）
```

#### 输出重定向指令:>和>>

```
1.覆盖: >
# echo "hello" > /home/hello.txt
2.追加: >>
# echo "hello" >> /home/hello.txt
- 基本语法:
# 1s -l > 文件 （功能描述：列表的内容写入文件中（覆盖写））
# ls -al >> 文件 （功能描述：列表的内容追加到文件内容的末尾）
# cat 文件1 > 文件2 （功能描述：将文件1的内容覆盖到文件2）
# echo "内容" >> 文件 （将内容追加到文件中）
- 应用实例:
案例1：
# ls -l /home > /home/index.txt : （说明:如果index.txt文件不存在，则会创建index.txt） 
案例2:
将当前日历信息追加到 /home/mycal文件中
# cal :显示当前日历信息
# cal >> /home/mycal
```

 #### ln指令：软链接也称为符号链接  ，类似于window的快捷方式,主要存放了链接其他文件的路径

```
- 基本语法:
# ln -s [源文件或目录] [软链接名] :给源文件创建一个软链接
# ln -s /root/  /home/myroot
```

#### history指令:查看历史指令

```
- 基本语法
# history : 所有
# history 10: 10条
- 执行历史指令
# ![指令编号] （行号）
```

