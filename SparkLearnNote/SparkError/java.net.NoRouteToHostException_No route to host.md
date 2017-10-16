# java.net.NoRouteToHostException: No route to host

错误信息：
    
    Application application_1504508652498_2831 failed 2 times due to Error launching appattempt_1504508652498_2831_000002. Got exception: java.net.NoRouteToHostException: No Route to Host from bigdata2.yiyun/192.168.37.22 to bigdata8.yiyun:45454 failed on socket timeout exception: java.net.NoRouteToHostException: No route to host; For more details see: http://wiki.apache.org/hadoop/NoRouteToHost
    at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
    at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
    at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
    at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
    at org.apache.hadoop.net.NetUtils.wrapWithMessage(NetUtils.java:791)
    at org.apache.hadoop.net.NetUtils.wrapException(NetUtils.java:757)
    at org.apache.hadoop.ipc.Client.call(Client.java:1473)
    at org.apache.hadoop.ipc.Client.call(Client.java:1400)
    at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:232)
    at com.sun.proxy.$Proxy85.startContainers(Unknown Source)
    at org.apache.hadoop.yarn.api.impl.pb.client.ContainerManagementProtocolPBClientImpl.startContainers(ContainerManagementProtocolPBClientImpl.java:96)
    at org.apache.hadoop.yarn.server.resourcemanager.amlauncher.AMLauncher.launch(AMLauncher.java:119)
    at org.apache.hadoop.yarn.server.resourcemanager.amlauncher.AMLauncher.run(AMLauncher.java:254)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
    at java.lang.Thread.run(Thread.java:745)
    Caused by: java.net.NoRouteToHostException: No route to host
    at sun.nio.ch.SocketChannelImpl.checkConnect(Native Method)
    at sun.nio.ch.SocketChannelImpl.finishConnect(SocketChannelImpl.java:739)
    at org.apache.hadoop.net.SocketIOWithTimeout.connect(SocketIOWithTimeout.java:206)
    at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:530)
    at org.apache.hadoop.net.NetUtils.connect(NetUtils.java:494)
    at org.apache.hadoop.ipc.Client$Connection.setupConnection(Client.java:608)
    at org.apache.hadoop.ipc.Client$Connection.setupIOstreams(Client.java:706)
    at org.apache.hadoop.ipc.Client$Connection.access$2800(Client.java:369)
    at org.apache.hadoop.ipc.Client.getConnection(Client.java:1522)
    at org.apache.hadoop.ipc.Client.call(Client.java:1439)
    ... 9 more
    . Failing the application.

查看防火墙状态

service iptables status

如果出现：

    表格：filter
    Chain INPUT (policy ACCEPT)
    num  target     prot opt source               destination         
    1    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED 
    2    ACCEPT     icmp --  0.0.0.0/0            0.0.0.0/0           
    3    ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0           
    4    ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           state NEW tcp dpt:22 
    5    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 
    
    Chain FORWARD (policy ACCEPT)
    num  target     prot opt source               destination         
    1    REJECT     all  --  0.0.0.0/0            0.0.0.0/0           reject-with icmp-host-prohibited 
    
    Chain OUTPUT (policy ACCEPT)
    num  target     prot opt source               destination   

则说明未关闭防火墙。

关闭防火墙：

service iptables stop

查看防火墙状态

service iptables status

此时则显示`iptables：未运行防火墙。`