# Docker常用操作

### 查看所有的images

    sudo docker images

### 查看正在运行的container

    sudo docker ps -a

### 停止container

    sudo docker stop 31474cadbb7f

### 删除container

    sudo docker rm 31474cadbb7f

### 删除images

    sudo docker rmi 3fa112fd3642

### 退出容器命令

退出容器命令`Ctrl+P+Q`。


### 进入容器命令

    sudo docker exec -it 782061a40d03  /bin/bash  


### 查看端口号命令

    netstat -tunlp|grep 8080