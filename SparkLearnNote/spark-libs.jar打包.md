#spark-libs.jar打包

服务器：slave6

spark 版本：2.1.0

将`$SPARK_HOME/jars`目录下所有的jar包，打包成一个jar文件，并将该文件上传到HDFS中。

在`spark-defaults.conf`文件中使用`spark.yarn.archive`参数将该jar包上传到HDFS中，
从而算短在spark-submit提交任务时上传jar包的时间。

命令如下：

    cd $SPARK_HOME 
    
    jar cv0f spark-libs-2.1.0.jar -C jars/ .
    
    cd conf/
    vi spark-defaults.conf
    #在spark-defaults.conf中添加：
    spark.yarn.archive=hdfs:///apps/spark/spark-libs-2.1.0.jar
    
    hadoop fs -put spark-libs-2.1.0.jar /apps/spark/
    hadoop fs -ls /apps/spark/