# spark链接MySQL报错“ (打开的文件过多)”

> 参考链接
> 
> 打开文件过多(Too many open files)
>  <http://support-it.huawei.com/docs/zh-cn/fusioninsight-all/maintenance-guide/zh-cn_topic_0045284818.html>
> 
> Spark-sql 运行提示too many open files
> <https://blog.csdn.net/wisdom_c_1010/article/details/46531519>

服务器：bigdata8
版本：contos 6.7
时间：2019年6月1日

执行ulimit -a命令查看有问题节点文件句柄数最多设置是多少，如果很小，建议修改成640000。

    ulimit -a

设置/etc/security/limits.conf

    vi /etc/security/limits.conf
    
在结尾# End of file下添加
    
    * soft nofile 65536
    * hard nofile 65536
    * soft nproc 65536
    * hard nproc 65536



解释：nofile---最大文件打开数
      nproc---最大进程数


重新打开一个终端，用ulimit -a命令查看是否修改成功，如果没有，请重新按照上述步骤重新修改。

    ulimit -a