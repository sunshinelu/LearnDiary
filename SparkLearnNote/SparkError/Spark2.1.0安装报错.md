# Spark 2.1.0 安装报错


在slave6上安装Spark 2.1.0 

问题1:

>
> 参考链接：
>
> https://www.iteblog.com/archives/1564

Caused by: java.lang.ClassNotFoundException: com.sun.jersey.api.client.config.ClientConfig


解决方法：

    cd /usr/hdp/current/hadoop-yarn-client/lib/
    ls jersey*
    cp jersey-core-1.9.jar /root/software/spark-2.1.0-bin-hadoop2.6/jars/
    cp jersey-client-1.9.jar /root/software/spark-2.1.0-bin-hadoop2.6/jars/
    cd /root/software/spark-2.1.0-bin-hadoop2.6/jars/
    ls jersey*
    mv jersey-client-2.22.2.jar temp.jersey-client-2.22.2.jar


测试spark-shell --master yarn 模式下是否能找到HBase依赖

    import org.apache.hadoop.hbase.HBaseConfiguration
    import org.apache.hadoop.hbase.client.Scan