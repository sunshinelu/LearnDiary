# ERROR XJ040: Failed to start database 'metastore_db'

> 参考链接：
> 
> hive 终端产生的问题 （Failed to start database 'metastore_db', see the next exception for details.）
> <https://blog.csdn.net/shaopeng5211/article/details/8679520>

错误描述：

在Jupyter中启动pyspark报错：

ERROR XJ040: Failed to start database 'metastore_db'

错误原因：

原因是在同一个目录下面 开启了2个终端。

解决方案：

重新启动启动pyspark