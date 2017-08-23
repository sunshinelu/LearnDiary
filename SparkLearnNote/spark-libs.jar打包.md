#spark-libs.jar打包

服务器：slave6

spark 版本：2.1.0


    cd $SPARK_HOME 
    
    jar cv0f spark-libs-2.1.0.jar -C jars/ .
    
    cd conf/
    vi spark-defaults.conf
    #在spark-defaults.conf中添加：
    spark.yarn.archive=hdfs:///apps/spark/spark-libs-2.1.0.jar
    
    hadoop fs -put spark-libs-2.1.0.jar /apps/spark/
    hadoop fs -ls /apps/spark/