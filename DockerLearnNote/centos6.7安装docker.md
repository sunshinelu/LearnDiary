# centos6.7安装docker


>
>
>
>
>
> Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
> <https://blog.csdn.net/edwzhang/article/details/41251015>


sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
  rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
  
  yum install docker-io
  
  [root@host-10-20-50-29 ~]# yum install docker-io
Loaded plugins: fastestmirror
Setting up Install Process
Loading mirror speeds from cached hostfile
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
[root@host-10-20-50-29 ~]# vi /etc/yum.repos.d/epel.repo

打开/etc/yum.repos.d/epel.repo，将

[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
修改为

[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch


yum clean all

yum install docker-io


Error: Cannot retrieve repository metadata (repomd.xml) for repository: upgrade. Please verify its path and try again