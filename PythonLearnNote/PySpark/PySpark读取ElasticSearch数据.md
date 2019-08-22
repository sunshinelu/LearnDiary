# PySpark读取ElasticSearch数据


> 参考链接：
>
> Spark SQL大数据处理并写入Elasticsearch
> <https://www.cnblogs.com/FG123/p/9748836.html>
> <https://github.com/a342058040/Spark-for-Python>
> <https://github.com/a342058040/Spark-for-Python/blob/master/spark_sql/spark_weather.py>
>
> pyspark访问Elasticsearch数据
> <https://blog.icocoro.me/2018/03/07/1803-pyspark-elasticsearch/>
>
>
### 1. 前置准备

- Spark版本：2.4.3
- pyspark版本：2.4.3
- Scala版本：2.11.8
- elasticsearch版本：6.2.3

- 所需jar包 `elasticsearch-spark-20_2.11-6.2.3.jar`

将  `elasticsearch-spark-20_2.11-6.2.3.jar`放到`$SPARK_HOME/jars`目录下.

如果不放在该目录下，会报错:
`py4j.protocol.Py4JJavaError: An error occurred while calling o46.save.
: java.lang.ClassNotFoundException: Failed to find data source: org.elasticsearch.spark.sql.`

如果将  `elasticsearch-spark-20_2.11-6.2.3.jar`放到`$SPARK_HOME/jars`目录下后，还是报错`Failed to find data source: org.elasticsearch.spark.sql`。
如果是在pycharm中测试代码，则重启pycharm。如果还是不行，则表示spark没有发现jar包，此时需重新编译pyspark。
使用如下命令进行编译：

    cd $SPARK_HOME/python 
    python setup.py sdist 
    pip install dist/*.tar.gz

** 注意，千万不能放到`$JAVA_HOME/jre/lib/ext`目录下面，如果放到该目录下在使用spark读取外部数据源时会报错`java.lang.NoClassDefFoundError: org/apache/spark/sql/sources/DataSourceRegister`。

因为系统会默认优先加载`$JAVA_HOME/jre/lib/ext`目录下面的jar包。




### 2. 演示案例

    
    from pyspark.sql import SparkSession
    from pyspark.sql.types import *
    from pyspark.sql import *
    
    import os
    # os.environ["PYSPARK_PYTHON"]="/Users/sunlu/anaconda2/envs/python36/bin/python3.6" #在Mac中使用
    
    
    spark = SparkSession\
            .builder\
            .appName("spark read and write es")\
            .getOrCreate()
    # save data to elasticsearch
    schema = StructType([
        StructField("id", StringType(), True),
        StructField("email", StringType(), True)
    ])
    
    data = [('1', 'ss'), ('2', 'dd'), ('3', 'ff_update'), ('4', '哈哈哈')]
    # data = [('10', 'ss'), ('20', 'dd'), ('30', 'ff_update'), ('40', '哈哈哈')]
    
    save_df = spark.createDataFrame(data, ['id', 'uname'])
    save_df.show(truncate=False)
    
    save_df.write \
        .format("org.elasticsearch.spark.sql") \
        .option("es.nodes", "127.0.0.1") \
        .option("es.resource", "pyspark_es/data") \
        .option("es.mapping.id", "id") \
        .mode('append') \
        .save()
    
    # read data from elasticsearch
    df = spark.read \
        .format("org.elasticsearch.spark.sql") \
        .option("es.nodes", "127.0.0.1") \
        .option("es.resource", "pyspark_es/data") \
        .load()
    
    df.show(truncate=False)
    
    spark.stop()