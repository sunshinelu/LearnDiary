# 在slave6上启动spark-shell的时候报错：java.util.NoSuchElementException: No value found for 'fair'

    17/08/20 10:16:18 WARN shortcircuit.DomainSocketFactory: The short-circuit local reads feature cannot be used because libhadoop cannot be loaded.
    17/08/20 10:16:22 ERROR scheduler.LiveListenerBus: Listener JobProgressListener threw an exception
    java.util.NoSuchElementException: No value found for 'fair'
        at scala.Enumeration.withName(Enumeration.scala:124)
        at org.apache.spark.ui.jobs.JobProgressListener$$anonfun$onEnvironmentUpdate$1.apply(JobProgressListener.scala:535)
        at org.apache.spark.ui.jobs.JobProgressListener$$anonfun$onEnvironmentUpdate$1.apply(JobProgressListener.scala:535)
        at scala.Option.map(Option.scala:146)
        at org.apache.spark.ui.jobs.JobProgressListener.onEnvironmentUpdate(JobProgressListener.scala:535)
        at org.apache.spark.scheduler.SparkListenerBus$class.doPostEvent(SparkListenerBus.scala:47)
        at org.apache.spark.scheduler.LiveListenerBus.doPostEvent(LiveListenerBus.scala:36)
        at org.apache.spark.scheduler.LiveListenerBus.doPostEvent(LiveListenerBus.scala:36)
        at org.apache.spark.util.ListenerBus$class.postToAll(ListenerBus.scala:63)
        at org.apache.spark.scheduler.LiveListenerBus.postToAll(LiveListenerBus.scala:36)
        at org.apache.spark.scheduler.LiveListenerBus$$anon$1$$anonfun$run$1$$anonfun$apply$mcV$sp$1.apply$mcV$sp(LiveListenerBus.scala:94)
        at org.apache.spark.scheduler.LiveListenerBus$$anon$1$$anonfun$run$1$$anonfun$apply$mcV$sp$1.apply(LiveListenerBus.scala:79)
        at org.apache.spark.scheduler.LiveListenerBus$$anon$1$$anonfun$run$1$$anonfun$apply$mcV$sp$1.apply(LiveListenerBus.scala:79)
        at scala.util.DynamicVariable.withValue(DynamicVariable.scala:58)
        at org.apache.spark.scheduler.LiveListenerBus$$anon$1$$anonfun$run$1.apply$mcV$sp(LiveListenerBus.scala:78)
        at org.apache.spark.util.Utils$.tryOrStopSparkContext(Utils.scala:1245)
        at org.apache.spark.scheduler.LiveListenerBus$$anon$1.run(LiveListenerBus.scala:77)
        
解决方案：

    
    