# key not found: _PYSPARK_DRIVER_CONN_INFO_PATH

## 1.报错：
    
    19/06/24 21:29:47 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    Exception in thread "main" java.util.NoSuchElementException: key not found: _PYSPARK_DRIVER_CONN_INFO_PATH
        at scala.collection.MapLike$class.default(MapLike.scala:228)
        at scala.collection.AbstractMap.default(Map.scala:59)
        at scala.collection.MapLike$class.apply(MapLike.scala:141)
        at scala.collection.AbstractMap.apply(Map.scala:59)
        at org.apache.spark.api.python.PythonGatewayServer$.main(PythonGatewayServer.scala:69)
        at org.apache.spark.api.python.PythonGatewayServer.main(PythonGatewayServer.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.spark.deploy.JavaMainApplication.start(SparkApplication.scala:52)
        at org.apache.spark.deploy.SparkSubmit.org$apache$spark$deploy$SparkSubmit$$runMain(SparkSubmit.scala:849)
        at org.apache.spark.deploy.SparkSubmit.doRunMain$1(SparkSubmit.scala:167)
        at org.apache.spark.deploy.SparkSubmit.submit(SparkSubmit.scala:195)
        at org.apache.spark.deploy.SparkSubmit.doSubmit(SparkSubmit.scala:86)
        at org.apache.spark.deploy.SparkSubmit$$anon$2.doSubmit(SparkSubmit.scala:924)
        at org.apache.spark.deploy.SparkSubmit$.main(SparkSubmit.scala:933)
        at org.apache.spark.deploy.SparkSubmit.main(SparkSubmit.scala)

>
> 参考链接
>
> python-sparksql 报错java.util.NoSuchElementException: key not found: _PYSPARK_DRIVER_CALLBACK_HOST
> <https://blog.csdn.net/PingChangYu/article/details/84666782>

当前pyspark版本是2.2.0，升级pyspark版本

    pip uninstall pyspark
    pip install pyspark
    
## 2.报错
    
    19/06/24 22:17:09 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    Traceback (most recent call last):
      File "D:/Workspace/PyCharm/Github/PythonDiary/Spark/DataBase/MySQL/pyspark_mysql_demo1.py", line 16, in <module>
        .option("password", "root") \
      File "D:\ProgramFiles\Anaconda3\lib\site-packages\pyspark\sql\readwriter.py", line 172, in load
        return self._df(self._jreader.load())
      File "D:\ProgramFiles\Anaconda3\lib\site-packages\py4j\java_gateway.py", line 1257, in __call__
        answer, self.gateway_client, self.target_id, self.name)
      File "D:\ProgramFiles\Anaconda3\lib\site-packages\pyspark\sql\utils.py", line 63, in deco
        return f(*a, **kw)
      File "D:\ProgramFiles\Anaconda3\lib\site-packages\py4j\protocol.py", line 328, in get_return_value
        format(target_id, ".", name), value)
    py4j.protocol.Py4JJavaError: An error occurred while calling o32.load.
    : java.sql.SQLException: No suitable driver
        at java.sql.DriverManager.getDriver(DriverManager.java:315)
        at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions$$anonfun$6.apply(JDBCOptions.scala:105)
        at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions$$anonfun$6.apply(JDBCOptions.scala:105)
        at scala.Option.getOrElse(Option.scala:121)
        at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions.<init>(JDBCOptions.scala:104)
        at org.apache.spark.sql.execution.datasources.jdbc.JDBCOptions.<init>(JDBCOptions.scala:35)
        at org.apache.spark.sql.execution.datasources.jdbc.JdbcRelationProvider.createRelation(JdbcRelationProvider.scala:32)
        at org.apache.spark.sql.execution.datasources.DataSource.resolveRelation(DataSource.scala:318)
        at org.apache.spark.sql.DataFrameReader.loadV1Source(DataFrameReader.scala:223)
        at org.apache.spark.sql.DataFrameReader.load(DataFrameReader.scala:211)
        at org.apache.spark.sql.DataFrameReader.load(DataFrameReader.scala:167)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at py4j.reflection.MethodInvoker.invoke(MethodInvoker.java:244)
        at py4j.reflection.ReflectionEngine.invoke(ReflectionEngine.java:357)
        at py4j.Gateway.invoke(Gateway.java:282)
        at py4j.commands.AbstractCommand.invokeMethod(AbstractCommand.java:132)
        at py4j.commands.CallCommand.execute(CallCommand.java:79)
        at py4j.GatewayConnection.run(GatewayConnection.java:238)
        at java.lang.Thread.run(Thread.java:748)
    
> 参考链接
>
> pyspark连接MySQL出错 java.sql.SQLException: No suitable driver
> <https://blog.csdn.net/helloxiaozhe/article/details/81027196>    

解决办法是将mysql的驱动jar包（mysql-connector-java-5.1.30.jar），放到C:\Program Files\Java\jdk1.8.0_131\jre\lib\ext目录下（JAVA_HOME目录下的jre\lib\ext目录下)

    C:\Users\sunlu>echo %JAVA_HOME%
    C:\Program Files\Java\jdk1.8.0_131