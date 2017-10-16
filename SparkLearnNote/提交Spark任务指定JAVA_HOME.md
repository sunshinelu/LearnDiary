# 提交Spark任务指定JAVA_HOME

> 参考链接
>
> 如何在Spark、MapReduce和Flink程序里面指定JAVA_HOME <https://www.iteblog.com/archives/1883.html>
>
>

Spark 2.2及其以上版本不再支持java7，但是目前集群中的java版本为7，由于使用集群的人数较多，不能轻易升级JD。

为了保证spark任务的正常提交，使用`spark.executorEnv.JAVA_HOME`和`spark.yarn.appMasterEnv.JAVA_HOME`，这分别为Spark的Executor和Driver指定JDK路径.

    spark-submit --master yarn-cluster  \
    --executor-memory 8g \
    --num-executors 80 \
    --queue iteblog \
    --conf "spark.yarn.appMasterEnv.JAVA_HOME=/home/iteblog/java/jdk1.8.0_25" \
    --conf "spark.executorEnv.JAVA_HOME=/home/iteblog/java/jdk1.8.0_25" \
    --executor-cores 1 \
    --class com.iteblog.UserActionParser /home/iteblog/spark/IteblogAction-1.0-SNAPSHOT.jar
    
也可以将这两个参数写到`$SPARK_HOME/conf/spark_default.conf`文件中