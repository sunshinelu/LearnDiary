#yarn-cluster提交任务报错

> 参考链接：
>
>yarn-cluster提交任务报错
>
><https://stackoverflow.com/questions/32341709/bad-substitution-when-submitting-spark-job-to-yarn-cluster>


报错：

    Stack trace: ExitCodeException exitCode=1: /mnt/yarn/nm/local/usercache/stack/appcache/application_1441066518301_0013/container_e03_1441066518301_0013_02_000001/launch_container.sh: line 24: $PWD:$PWD/__hadoop_conf__:$PWD/__spark__.jar:$HADOOP_CONF_DIR:/usr/hdp/current/hadoop-client/*:/usr/hdp/current/hadoop-client/lib/*:/usr/hdp/current/hadoop-hdfs-client/*:/usr/hdp/current/hadoop-hdfs-client/lib/*:/usr/hdp/current/hadoop-yarn-client/*:/usr/hdp/current/hadoop-yarn-client/lib/*:$PWD/mr-framework/hadoop/share/hadoop/mapreduce/*:$PWD/mr-framework/hadoop/share/hadoop/mapreduce/lib/*:$PWD/mr-framework/hadoop/share/hadoop/hdfs/lib/*:$PWD/mr-framework/hadoop/share/hadoop/tools/lib/*:/usr/hdp/${hdp.version}/hadoop/lib/hadoop-lzo-0.6.0.${hdp.version}.jar:/etc/hadoop/conf/secure: bad substitution

    at org.apache.hadoop.util.Shell.runCommand(Shell.java:545)
    at org.apache.hadoop.util.Shell.run(Shell.java:456)
    at org.apache.hadoop.util.Shell$ShellCommandExecutor.execute(Shell.java:722)
    at org.apache.hadoop.yarn.server.nodemanager.DefaultContainerExecutor.launchContainer(DefaultContainerExecutor.java:211)
    at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:302)
    at org.apache.hadoop.yarn.server.nodemanager.containermanager.launcher.ContainerLaunch.call(ContainerLaunch.java:82)
    at java.util.concurrent.FutureTask.run(FutureTask.java:266)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
    at java.lang.Thread.run(Thread.java:745)
    
解决方案：

在$SPARK_HOME/conf目录下新建`java-opts`文件

    vi java-opts 
    
文件中内容为：

    -Dhdp.version=2.2.4.2-2


`spark-defaults.conf`文件中内容为：

    spark.driver.extraJavaOptions -Dhdp.version=current
    spark.yarn.am.extraJavaOptions -Dhdp.version=current 