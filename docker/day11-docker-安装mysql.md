```
docker run -id \
-p 3307:3306 \
--name=db_mysql \
-v /root/mysql/conf:/etc/mysql/conf.d \
-v /root/mysql/logs:/logs \
-v /root/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
--restart=always \
mysql:8.0 \
--lower_case_table_names=1
```

