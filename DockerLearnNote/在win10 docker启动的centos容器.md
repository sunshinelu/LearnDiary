# 在win10 docker启动的centos 6.7容器

时间：2019年4月27日

> 参考链接
>
> 在win10 docker启动的centos容器中安装nginx
> <https://www.cnblogs.com/gujianzhe/p/9317126.html>
>
>
>
>


1. 从阿里云镜像服务中拉取一个centos镜像：


    docker pull registry.cn-hangzhou.aliyuncs.com/max/centos6.7-docker

2. 启动容器：
    
    
    docker run --name my-cnt-centos-6.7  -itd -p 80:80  registry.cn-hangzhou.aliyuncs.com/max/centos6.7-docker /bin/bash

3. 查看运行的容器


    C:\Users\sunlu>docker ps
    CONTAINER ID        IMAGE                                                    COMMAND             CREATED             STATUS              PORTS                                                          NAMES
    d9abffd4f7ac        registry.cn-hangzhou.aliyuncs.com/max/centos6.7-docker   "/bin/bash"         17 seconds ago      Up 15 seconds       21-22/tcp, 3306/tcp, 6379/tcp, 11211/tcp, 0.0.0.0:80->80/tcp   my-cnt-centos-6.7

4. 进入这个容器

    
    docker exec -it d9abffd4f7ac  /bin/bash