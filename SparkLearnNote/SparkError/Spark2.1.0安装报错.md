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

配置spark-defaults.conf文件：

    vi spark-defaults.conf
    
    spark.serializer                 org.apache.spark.serializer.KryoSerializer
    spark.driver.memory              5g
    
    spark.driver.extraJavaOptions -Dhdp.version=current
    spark.yarn.am.extraJavaOptions -Dhdp.version=current
    spark.yarn.archive=hdfs:///apps/spark/spark-libs-2.1.0.jar
    
    spark.shuffle.consolidateFiles=true
    spark.speculation=true
    spark.scheduler.mode=fair
    spark.network.timeout=300
    spark.shuffle.file.buffer=64k
    spark.reducer.maxSizeInFlight=96M
    spark.shuffle.io.maxRetries=60
    spark.shuffle.io.retryWait=60s
    spark.shuffle.memoryFraction=0.3
    spark.shuffle.sort.bypassMergeThreshold=300
    
    #write.csv等方法写出到hdfs的文件，中文乱码
    spark.executor.extraJavaOptions -Dfile.encoding=UTF-8

配置spark-env.sh文件：
    
    vi spark-env.sh
    
    export JAVA_HOME=/usr/jdk64/jdk1.7.0_67
    export SPARK_HOME=/root/software/spark-2.1.0-bin-hadoop2.6
    export HADOOP_HOME=/usr/hdp/current/hadoop-client
    export HBASE_HOME=/usr/hdp/current/hbase-client
    export HIVE_HOME=/usr/hdp/current/hive-client
    export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$HIVE_HOME/bin:$SPARK_HOME/bin
    export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
    export SPARK_CONF_DIR=${SPARK_HOME:-/usr/hdp/current/spark-client}/conf
    export SPARK_LOG_DIR=/var/log/spark
    export SPARK_PID_DIR=/var/run/spark
    SPARK_IDENT_STRING=$USER
    SPARK_NICENESS=0
    export HADOOP_CONF_DIR=/usr/hdp/current/hadoop-client/conf
    export YARN_CONF_DIR=/usr/hdp/current/hadoop-yarn-client/etc/hadoop
    export SPARK_LIBARY_PATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$HADOOP_HOME/lib/native:$HBASE_HOME/lib
    
    export SPARK_CLASSPATH=:/root/software/extraClass/ansj_seg-3.7.6-all-in-one.jar:/root/software/extraClass/mysql-connector-java-5.1.17.jar:/usr/hdp/2.2.4.2-2/zookeeper/lib/jsoup-1.7.1.jar:/usr/hdp/current/hbase-client/lib/hbase-client-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/hbase-server-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/hbase-common-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/hbase-protocol-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/htrace-core-2.04.jar:/usr/hdp/current/hbase-client/lib/hbase-hadoop-compat-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/hbase-it-0.98.4.2.2.4.2-2-hadoop2.jar:/usr/hdp/current/hbase-client/lib/guava-12.0.1.jar
    
    export SPARK_WORKER_MEMORY=6g

添加java-opts文件：

    vi java-opts 
    
    -Dhdp.version=2.2.4.2-2

测试spark-shell --master yarn 模式下是否能找到HBase依赖

    import org.apache.hadoop.hbase.HBaseConfiguration
    import org.apache.hadoop.hbase.client.Scan