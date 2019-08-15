# Ubuntu16.04下部署Oracle docker环境

> 参考链接
> 
> Get Docker CE for Ubuntu
> <https://docs.docker.com/install/linux/docker-ce/ubuntu/>
>
> 【链接】Docker拉取oracle11g镜像配置
>  <https://blog.csdn.net/qq_38380025/article/details/80647620>
>
> Docker 下拉取oracle 11g镜像配置
> <https://blog.csdn.net/zwx521515/article/details/77982884>
>
> ubuntu docker 安装 oracle
> <https://www.cnblogs.com/linhuchong1/articles/9441733.html>
> 
> Docker 删除image
> <https://blog.csdn.net/byc233518/article/details/77434724>
>
> docker使用阿里云Docker镜像库加速(修订版)
> <https://blog.csdn.net/bwlab/article/details/50542261>
>
> 如何查看oracle客户端是32位还是64位
> <https://jingyan.baidu.com/article/363872ec1ad61b6e4ba16fb4.html>

系统：Ubuntu 16.04.6 LTS 
服务器：10.20.5.68
安装时间：2019年4月27日



## 1、ubuntu 安装docker


1) uninstall older version docker

    
    sudo apt-get remove docker docker-engine docker.io containerd runc

2) Update the `apt` package index:


    sudo apt-get update

3) Install packages to allow apt to use a repository over HTTPS


    sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

4) Add Docker’s official GPG key:


    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -


    sudo apt-key fingerprint 0EBFCD88
    
    
5) set up the stable repository
    

    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

6) INSTALL DOCKER CE
    
 
    sudo apt-get update
    
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    
    sudo docker run hello-world
    
    sudo docker ps -a

docker 安装完成。


## 2、docker中安装oracle


1) 开始拉取镜像-执行命令：


    sudo docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

下载完成后 查看镜像：

    user@user-HVM-domU:~$ sudo docker images
    REPOSITORY                                             TAG                 IMAGE ID            CREATED             SIZE
    registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g   latest              3fa112fd3642        3 years ago         6.85GB
    user@user-HVM-domU:~$ 

2) 创建容器


    sudo docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g

输入命令后显示如下：
    
    user@user-HVM-domU:~$ sudo docker run -d -p 1521:1521 --name oracle11g registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g
    31474cadbb7f6389c8b8d37b046cd946871014d9931961f76057fed6f134ab38

3) 启动容器 


    sudo docker start oracle11g
 
输入命令后显示如下：

    user@user-HVM-domU:~$ sudo docker start oracle11g
    oracle11g


4) 进入镜像进行配置


    sudo docker exec -it oracle11g bash

输入命令后显示如下：

    user@user-HVM-domU:~$ sudo docker exec -it oracle11g bash
    [oracle@31474cadbb7f /]$ 

进行软连接

    [oracle@31474cadbb7f /]$ sqlplus /nolog
    bash: sqlplus: command not found

发现没有这个命令，用不了

    [oracle@31474cadbb7f /]$ su root
    Password: helowin
    
密码是：helowin

编辑profile文件配置ORACLE环境变量

    [root@31474cadbb7f /]# vi /etc/profile

在最后加上

    
    export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
     
    export ORACLE_SID=helowin
     
    export PATH=$ORACLE_HOME/bin:$PATH
    
创建软连接    

    [root@31474cadbb7f /]# ln -s $ORACLE_HOME/bin/sqlplus /usr/bin

切换到oracle 用户

    [root@31474cadbb7f /]# su - oracle
    
 注意：一定要写中间的内条 -   必须要，否则软连接无效 
 
5) 登录sqlplus并修改sys、system用户密码

输入以下内容：

    sqlplus /nolog
    
    conn /as sysdba
    
    alter user system identified by system;
    
    alter user sys identified by sys;
    
    ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
    
    exit

显示如下：

    [oracle@31474cadbb7f ~]$ sqlplus /nolog

    SQL*Plus: Release 11.2.0.1.0 Production on Sat Apr 27 18:14:24 2019
    
    Copyright (c) 1982, 2009, Oracle.  All rights reserved.

    SQL> conn /as sysdba
    Connected.
    SQL> alter user system identified by system;
    
    User altered.
    
    SQL> alter user sys identified by sys;
    
    User altered.
    
    SQL> LTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
    SP2-0734: unknown command beginning "LTER PROFI..." - rest of line ignored.
    SQL> ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
    
    Profile altered.
    
    SQL> exit
    Disconnected from Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
    With the Partitioning, OLAP, Data Mining and Real Application Testing options


6) oracle 的 lsnrctl 服务

    
    [oracle@31474cadbb7f ~]$ lsnrctl status
    
    LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 27-APR-2019 18:21:27
    
    Copyright (c) 1991, 2009, Oracle.  All rights reserved.
    
    Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
    Start Date                27-APR-2019 18:10:32
    Uptime                    0 days 0 hr. 10 min. 55 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Parameter File   /home/oracle/app/oracle/product/11.2.0/dbhome_2/network/admin/listener.ora
    Listener Log File         /home/oracle/app/oracle/diag/tnslsnr/31474cadbb7f/listener/alert/log.xml
    Listening Endpoints Summary...
      (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
      (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=31474cadbb7f)(PORT=1521)))
    Services Summary...
    Service "helowin" has 1 instance(s).
      Instance "helowin", status READY, has 1 handler(s) for this service...
    Service "helowinXDB" has 1 instance(s).
      Instance "helowin", status READY, has 1 handler(s) for this service...
    The command completed successfully
    [oracle@31474cadbb7f ~]$ 

7) Oracle配置如下：


    IP：10.20.5.68
    
    端口号：1521
    
    服务名：helowinXDB 或者 helowin
    
    用户名：system
    
    密码：system

查看 oracle 是32位还是64位

    su root
    密码：helowin
    
    yum -y install file
    
    file /home/oracle/app/oracle/product/11.2.0/dbhome_2/bin/sqlplus

----------

    sudo apt-get update

    sudo apt-get docker.io

    docker -v
    ＃Docker version 18.09.2, build 6247962
    
    docker search oracle
    
    docker pull jaspeen/oracle-11g

操作步骤

    user@user-HVM-domU:~$ sudo docker run -d -p 7070:8080 -p 1521:1521 jaspeen/oracle-11g
    [sudo] password for user: 
    d31df5289fb2a1d0e44609513e8c3c3029975efaa727e2962bddb451bd9ec12b
    
    
    user@user-HVM-domU:~$ sudo docker ps -a
    CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS                      PORTS               NAMES
    d31df5289fb2        jaspeen/oracle-11g   "/assets/entrypoint.…"   54 seconds ago      Exited (1) 52 seconds ago                       inspiring_diffie
    c0c93f3e0f95        hello-world          "/hello"                 9 hours ago         Exited (0) 9 hours ago                          nostalgic_torvalds
    user@user-HVM-domU:~$ 


    user@user-HVM-domU:~$ sudo docker start d31df5289fb2
    d31df5289fb2

    user@user-HVM-domU:~$ sudo docker start d31df5289fb2
    d31df5289fb2

    user@user-HVM-domU:~$ sudo docker exec -ti c0c93f3e0f95 /bin/bash
    
    
 