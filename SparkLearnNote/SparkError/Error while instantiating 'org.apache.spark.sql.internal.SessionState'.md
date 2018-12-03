# Error while instantiating 'org.apache.spark.sql.internal.SessionState'

在inglory项目中，无法初始化spark，错误信息如下：
    
    objc[96629]: Class JavaLaunchHelper is implemented in both /Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/bin/java and /Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home/jre/lib/libinstrument.dylib. One of the two will be used. Which one is undefined.
    2018-12-03 14:14:12,701 WARN  [org.apache.hadoop.util.NativeCodeLoader] - Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Exception in thread "main" java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.internal.SessionState':
        at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:981)
        at org.apache.spark.sql.SparkSession.sessionState$lzycompute(SparkSession.scala:110)
        at org.apache.spark.sql.SparkSession.sessionState(SparkSession.scala:109)
        at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
        at org.apache.spark.sql.SparkSession$Builder$$anonfun$getOrCreate$5.apply(SparkSession.scala:878)
        at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
        at scala.collection.mutable.HashMap$$anonfun$foreach$1.apply(HashMap.scala:99)
        at scala.collection.mutable.HashTable$class.foreachEntry(HashTable.scala:230)
        at scala.collection.mutable.HashMap.foreachEntry(HashMap.scala:40)
        at scala.collection.mutable.HashMap.foreach(HashMap.scala:99)
        at org.apache.spark.sql.SparkSession$Builder.getOrCreate(SparkSession.scala:878)
        at SparkVersion$.main(SparkVersion.scala:8)
        at SparkVersion.main(SparkVersion.scala)
    Caused by: java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:422)
        at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:978)
        ... 12 more
    Caused by: java.util.ServiceConfigurationError: org.apache.hadoop.fs.FileSystem: Provider org.apache.hadoop.fs.s3a.S3AFileSystem could not be instantiated
        at java.util.ServiceLoader.fail(ServiceLoader.java:232)
        at java.util.ServiceLoader.access$100(ServiceLoader.java:185)
        at java.util.ServiceLoader$LazyIterator.nextService(ServiceLoader.java:384)
        at java.util.ServiceLoader$LazyIterator.next(ServiceLoader.java:404)
        at java.util.ServiceLoader$1.next(ServiceLoader.java:480)
        at org.apache.hadoop.fs.FileSystem.loadFileSystems(FileSystem.java:2563)
        at org.apache.hadoop.fs.FileSystem.getFileSystemClass(FileSystem.java:2574)
        at org.apache.hadoop.fs.FileSystem.createFileSystem(FileSystem.java:2591)
        at org.apache.hadoop.fs.FileSystem.access$200(FileSystem.java:91)
        at org.apache.hadoop.fs.FileSystem$Cache.getInternal(FileSystem.java:2630)
        at org.apache.hadoop.fs.FileSystem$Cache.get(FileSystem.java:2612)
        at org.apache.hadoop.fs.FileSystem.get(FileSystem.java:370)
        at org.apache.hadoop.fs.Path.getFileSystem(Path.java:296)
        at org.apache.spark.sql.catalyst.catalog.InMemoryCatalog.liftedTree1$1(InMemoryCatalog.scala:110)
        at org.apache.spark.sql.catalyst.catalog.InMemoryCatalog.createDatabase(InMemoryCatalog.scala:108)
        at org.apache.spark.sql.internal.SharedState.<init>(SharedState.scala:99)
        at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
        at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
        at scala.Option.getOrElse(Option.scala:121)
        at org.apache.spark.sql.SparkSession.sharedState$lzycompute(SparkSession.scala:101)
        at org.apache.spark.sql.SparkSession.sharedState(SparkSession.scala:100)
        at org.apache.spark.sql.internal.SessionState.<init>(SessionState.scala:157)
        ... 17 more
    Caused by: java.lang.NoClassDefFoundError: com/amazonaws/auth/AWSCredentialsProvider
        at java.lang.Class.getDeclaredConstructors0(Native Method)
        at java.lang.Class.privateGetDeclaredConstructors(Class.java:2671)
        at java.lang.Class.getConstructor0(Class.java:3075)
        at java.lang.Class.newInstance(Class.java:412)
        at java.util.ServiceLoader$LazyIterator.nextService(ServiceLoader.java:380)
        ... 36 more
    Caused by: java.lang.ClassNotFoundException: com.amazonaws.auth.AWSCredentialsProvider
        at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        ... 41 more
    


解决方案：

删除  `hadoop-aws-2.6.0.jar` 这个jar包。
