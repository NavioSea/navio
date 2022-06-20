## 			Docker 部署DEVOPS

### 			1.1基于Docker部署GitLab

1.部署本地GitLab

相关文章:https://www.csdn.net/tags/MtTaEgxsNjA3ODg0LWJsb2cO0O0O.html

* docker pull gitlab-ce-版本

* 编写脚本

  ```
   bash
  GITLAB_HOEM=/opt/gitlab
  IMAGE_NAME=gitlab/gitlab-ce:11.0.1-ce.0
  mkdir -p ${GITLAB_HOEM}/conf
  mkdir -p ${GITLAB_HOEM}/logs
  mkdir -p ${GITLAB_HOEM}/data
  
  chmod 777 ${GITLAB_HOEM} -R
  docker pull ${IMAGE_NAME}
  
  docker run -d --publish 444:443 --publish 8789:80 --publish 222:22 --name mygitlab --restart always --volume $GITLAB_HOEM/config:/etc/gitlab --volume $GITLAB_HOEM/logs:/var/log/gitlab --volume $GITLAB_HOEM/data:/var/opt/gitlab  ${IMAGE_NAME}
  
  ```

* 执行脚本,等待启动完成

2.创建gitlab备份(data目录)

`docker exec -t <container name> gitlab-backup create`

![image-20220609231509603](C:\Users\lzh\AppData\Roaming\Typora\typora-user-images\image-20220609231509603.png)

## Sonarqube部署

1.基本介绍