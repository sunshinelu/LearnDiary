# Spark2.1.0单机模式无法启动master的问题

> 参考链接
> 
> 解决java.net.UnknownHostException: 主机名: 主机名: 未知的名称或服务
> <https://blog.csdn.net/huanbia/article/details/69055523>
>
> Spark2.1.0单机模式无法启动master的问题
> <https://www.cnblogs.com/xyliao/p/6502747.html>
>
> 单机运行Spark Shell遇到的一个低级错误
> <https://blog.csdn.net/chengyuqiang/article/details/69665878>

在测试环境10.20.5.116安装spark2.1.0

在启动spark-shell时，报错：

`java.net.UnknownHostException:主机名：主机名:未知的名称或服务错误 `

解决方案

修改hosts文件

    vi /etc/hosts
    
    10.20.5.116 host-10-20-88-132
    10.20.88.132 host-10-20-88-132

为什么要配两个呢，因为10.20.5.116 是内外I，而10.20.88.132是 `ifconfig `显示的结果。
    
    [root@host-10-20-88-132 conf]# ifconfig 
    eth2      Link encap:Ethernet  HWaddr FA:16:3E:1E:85:41  
              inet addr:10.20.88.132  Bcast:10.20.88.255  Mask:255.255.255.0
              inet6 addr: fe80::f816:3eff:fe1e:8541/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:30213940 errors:0 dropped:0 overruns:0 frame:0
              TX packets:14123400 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000 
              RX bytes:29523838170 (27.4 GiB)  TX bytes:2547715476 (2.3 GiB)
    
    lo        Link encap:Local Loopback  
              inet addr:127.0.0.1  Mask:255.0.0.0
              inet6 addr: ::1/128 Scope:Host
              UP LOOPBACK RUNNING  MTU:65536  Metric:1
              RX packets:14548 errors:0 dropped:0 overruns:0 frame:0
              TX packets:14548 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1 
              RX bytes:732608 (715.4 KiB)  TX bytes:732608 (715.4 KiB)
    

    
    
    
然后再启动spark-shell时，报错：

`无法指定被请求的地址: Service 'sparkDriver' failed after 16 retries (`

解决方案：

在spark-env.sh中配置：

    export  SPARK_MASTER_HOST=127.0.0.1
    export  SPARK_LOCAL_IP=127.0.0.1