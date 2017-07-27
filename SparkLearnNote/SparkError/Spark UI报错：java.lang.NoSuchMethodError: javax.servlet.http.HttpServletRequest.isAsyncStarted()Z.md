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


双击`shift`键，搜索`HttpServletResponse`方法，显示如下信息：


![图一](/Users/sunlu/Workspaces/PyCharm/Github/LearnDiary/images/Spark/spark.error.01.png)


发现在servlet-api-2.5.jar中确实不存在该方法，问题可想而知，servlet-api-3.1.0.jar中的方法被覆盖。

## 解决方法：

> 参考链接：
<http://blog.csdn.net/ainidong2005/article/details/53088957>


删除javax.servlet:servlet-api:2.5和org.mortbay.jetty:servlet-api-2.5:6.1.14。

![图二](/Users/sunlu/Workspaces/PyCharm/Github/LearnDiary/images/Spark/spark.error.02.png)

选中`javax.servlet:servlet-api:2.5`，单击鼠标右键，选择`Open Libray Settings`，可以看到上图结果，点击`－`进行删除该依赖。

只删除该依赖仍然无法打开spark ui，让然报错：
17/07/27 10:21:45 WARN HttpChannel: /jobs/
java.lang.NoSuchMethodError: javax.servlet.http.HttpServletRequest.isAsyncStarted()Z


![图三](/Users/sunlu/Workspaces/PyCharm/Github/LearnDiary/images/Spark/spark.error.03.png)

删除org.mortbay.jetty:servlet-api-2.5:6.1.14之后Spark UI显示正常。

**注意：**

![图四](/Users/sunlu/Workspaces/PyCharm/Github/LearnDiary/images/Spark/spark.error.04.png)


ort.glassfish.jersey.containers:jersey-container-servlet-cor:2.17该依赖不可以删除，否则程序会出错。

    Caused by: java.lang.ClassNotFoundException: org.glassfish.jersey.servlet.ServletContainer

若不慎删除，通过对`pom.xml`进行`Reimport`操作即可重新倒入该依赖，同时，之前删除的依赖也会重新导入。
