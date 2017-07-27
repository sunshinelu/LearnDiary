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



![Al](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/t1.png)

![t1](https://github.com/sunshinelu/LearnDiary/blob/master/images/Spark/spark.error.01.png)

![](file:///Users/sunlu/Documents/workspace/IDEA/Github/LearnDiary/images/Spark/spark.error.01.png) 

![CSDN图标](http://imgtech.gmw.cn/attachement/jpg/site2/20111223/f04da22d7ba7105e1d7507.jpg "这是CSDN的图标")


![GitHub](https://avatars2.githubusercontent.com/u/3265208?v=3&s=100 "GitHub,Social Coding")