# Spark UI报错：java.lang.NoSuchMethodError: javax.servlet.http.HttpServletRequest.isAsyncStarted()Z


错误信息：

17/07/26 21:42:32 WARN HttpChannel: /jobs/
java.lang.NoSuchMethodError: javax.servlet.http.HttpServletRequest.isAsyncStarted()Z
	at org.spark_project.jetty.servlets.gzip.GzipHandler.handle(GzipHandler.java:484)
	at org.spark_project.jetty.server.handler.ContextHandlerCollection.handle(ContextHandlerCollection.java:215)
	at org.spark_project.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:97)
	at org.spark_project.jetty.server.Server.handle(Server.java:499)
	at org.spark_project.jetty.server.HttpChannel.handle(HttpChannel.java:311)
	at org.spark_project.jetty.server.HttpConnection.onFillable(HttpConnection.java:257)
	at org.spark_project.jetty.io.AbstractConnection$2.run(AbstractConnection.java:544)
	at org.spark_project.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:635)



错误信息为`java.lang.NoSuchMethodError: javax.servlet.http.HttpServletRequest.isAsyncStarted()Z`找不到HttpServletRequest方法，
通过双击`shift`键查找“”发现
tu1
![pic1](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/spark.error.01.png)







![pic1](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/spark.error.02.png)





![pic1](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/spark.error.03.png)




![pic1](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/spark.error.04.png)


参考链接：
<http://blog.csdn.net/ainidong2005/article/details/53088957>