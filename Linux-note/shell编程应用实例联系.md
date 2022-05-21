![image-20220521153217491](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220521153217491.png)

#### 数据库备份步骤

```
#!/bin/bash
#备份目录
BACKUP=/data/backup/db
#当前时间
DATETIME=$(date +%Y-%m-%d_%H%M%S)
echo "开始执行备份的时间为 $DATETIME"
#数据库的地址
HOST=localhost
#数据库用户名
DB_USER=root
#数据库密码
DB_PW=Admin$123
#备份数据库名
DATABASE=fhexam
#创建备份目录,如果不存在,创建
[ ! -d "${BACKUP}/${DATETIME}" ] && mkdir -p "${BACKUP}/${DATETIME}"
#备份数据库
mysqldump -u${DB_USER} -P${DB_PW} --host=${HOST} -q -R --databases ${DATABASE} | gzip > ${BACKUP}/${DATETIME}/${DATETIME}.sql.gz
#将文件处理成tar.gz
cd ${BACKUP} 
tar -zcvf $DATETIME.tar.gz ${DATETIME}
#删除对应的备份目录
rm -rf ${BACKUP}/${DATETIME}
#删除10天前的备份文件(-exec rm -rf {} \;//将前面得到的文件删除)
find ${BACKUP} -atime +10 -name "*.tar.gz" -exec rm -rf {} \;
echo "备份数据库$DATABASE 成功~"
```

