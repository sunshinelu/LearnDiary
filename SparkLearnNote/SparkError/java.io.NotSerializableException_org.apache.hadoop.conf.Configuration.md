# java.io.NotSerializableException: org.apache.hadoop.conf.Configuration

> 参考链接
>
> SparkTask未序列化(Tasknotserializable)问题分析
> http://blog.csdn.net/javastart/article/details/51206715


直接在spark－shell下直接运行时会出现如下错误：
    
    Caused by: java.io.NotSerializableException: org.apache.hadoop.conf.Configuration
   
解决方案：在`val conf = HBaseConfiguration.create()`前面添加`@transient`，即：

    @transient val conf = HBaseConfiguration.create()
