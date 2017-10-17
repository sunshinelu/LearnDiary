# Spark Shuffle FetchFailedException

> 参考链接
> Spark Shuffle FetchFailedException解决方案 <http://blog.csdn.net/lsshlsw/article/details/51213610>
>
>
>
>

spark shuffle报错：

    org.apache.spark.shuffle.MetadataFetchFailedException: 
    Missing an output location for shuffle 0

和

    org.apache.spark.shuffle.FetchFailedException:
    Failed to connect to hostname/192.168.xx.xxx:50268
    
在spark-submit时可以使用的解决方案：

1. 调整`spark.sql.shuffle.partitions`参数。`spark.sql.shuffle.partitions`为分区数，默认值是200.
shuffle严重时适度提高该值。

2. 调整`spark.default.parallelism`参数。`spark.default.parallelism`控制shuffle read与reduce处理的分区数，默认为运行任务的core的总数。
官方建议为设置成运行任务的core的2-3倍。

3. 调整`spark.executor.memory`值。适当提高executor的memory值。