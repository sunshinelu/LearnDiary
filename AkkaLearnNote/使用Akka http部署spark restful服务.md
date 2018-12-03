# 使用Akka http部署spark restful服务

> 参考链接
> 
> spark程序运行时问题 <http://www.aiuxian.com/article/p-1217527.html>
> 
> spark-as-service-using-embedded-server <https://github.com/spoddutur/spark-as-service-using-embedded-server>
> 

将项目打成jar包，使用 `java －jar XXXXXX.jar`执行的时候报如下错误：

    没有主清单属性

或者

    Error: A JNI error has occurred, please check your installation and try again
    Exception in thread "main" java.lang.SecurityException: Invalid signature file digest for Manifest main attributes

解决方案：

使用shade插件打包，打包的时候指定`mainClass`、`resource`并`exclude`以 `*.SF`、`*.DSA`、`*.RSA`结尾的文件。

build 信息可参考

     <build>
            <resources>
                <resource>
                    <!-- //这个地方是把resources下的配置文件打进包里-->
                    <directory>src/main/resources</directory>
                </resource>
            </resources>
            <sourceDirectory>src</sourceDirectory>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                        <encoding>UTF-8</encoding>
                    </configuration>
                </plugin>
    
    
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.2</version>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                            <configuration>
                                <filters>
                                    <filter>
                                        <artifact>*:*</artifact>
                                        <excludes>
                                            <exclude>META-INF/*.SF</exclude>
                                            <exclude>META-INF/*.DSA</exclude>
                                            <exclude>META-INF/*.RSA</exclude>
                                        </excludes>
                                    </filter>
                                </filters>
                                <transformers>
                                    <transformer
                                            implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                        <mainClass>com.evayInfo.Inglory.data.mining.platform.boot</mainClass>
                                    </transformer>
                                    <transformer
                                            implementation="org.apache.maven.plugins.shade.resource.AppendingTransformer">
                                        <resource>reference.conf</resource>
                                    </transformer>
                                </transformers>
                            </configuration>
                        </execution>
                    </executions>
                </plugin>
    
                <!--<plugin>-->
                    <!--<artifactId>maven-assembly-plugin</artifactId>-->
                    <!--<version>2.3</version>-->
                    <!--<configuration>-->
                        <!--<classifier>dist</classifier>-->
                        <!--<appendAssemblyId>true</appendAssemblyId>-->
                        <!--<archive>-->
                            <!--<manifest>-->
                                <!--<mainClass>com.evayInfo.Inglory.data.mining.platform.boot</mainClass>-->
                            <!--</manifest>-->
                        <!--</archive>-->
                        <!--<descriptorRefs>-->
                            <!--<descriptor>jar-with-dependencies</descriptor>-->
                        <!--</descriptorRefs>-->
                    <!--</configuration>-->
                    <!--<executions>-->
                        <!--<execution>-->
                            <!--<id>make-assembly</id>-->
                            <!--<phase>package</phase>-->
                            <!--<goals>-->
                                <!--<goal>single</goal>-->
                            <!--</goals>-->
                        <!--</execution>-->
                    <!--</executions>-->
                <!--</plugin>-->
    
                <plugin>
                    <groupId>org.scala-tools</groupId>
                    <artifactId>maven-scala-plugin</artifactId>
                    <version>2.15.2</version>
                    <executions>
                        <execution>
                            <goals>
                                <goal>compile</goal>
                                <goal>testCompile</goal>
                            </goals>
                        </execution>
                    </executions>
                    <configuration>
                        <scalaVersion>${scala.version}</scalaVersion>
                        <args>
                            <arg>-target:jvm-1.8</arg>
                        </args>
                    </configuration>
                </plugin>
            </plugins>
      </build>


打包完成中，使用如下命令运行jar包

`java -jar target/DataMining-1.0-SNAPSHOT.jar `

服务可以启动，但是spark无法初始化，错误信息如下：
    
    [ERROR] [12/03/2018 12:44:23.090] [default-akka.actor.default-dispatcher-4] [akka.actor.ActorSystemImpl(default)] Error during processing of request: 'Error while instantiating 'org.apache.spark.sql.internal.SessionState':'. Completing with 500 Internal Server Error response. To change default exception handling behavior, provide a custom ExceptionHandler.
    java.lang.IllegalArgumentException: Error while instantiating 'org.apache.spark.sql.internal.SessionState':
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
        at com.evayInfo.Inglory.data.mining.platform.routes.SparkServices$$anonfun$versionRoute$1$$anonfun$apply$1$$anonfun$apply$2.apply(SparkServices.scala:59)
        at com.evayInfo.Inglory.data.mining.platform.routes.SparkServices$$anonfun$versionRoute$1$$anonfun$apply$1$$anonfun$apply$2.apply(SparkServices.scala:30)
        at akka.http.scaladsl.server.Directive$$anonfun$addByNameNullaryApply$1$$anonfun$apply$13.apply(Directive.scala:134)
        at akka.http.scaladsl.server.Directive$$anonfun$addByNameNullaryApply$1$$anonfun$apply$13.apply(Directive.scala:134)
        at akka.http.scaladsl.server.ConjunctionMagnet$$anon$2$$anonfun$apply$14$$anonfun$apply$15$$anonfun$apply$16.apply(Directive.scala:162)
        at akka.http.scaladsl.server.ConjunctionMagnet$$anon$2$$anonfun$apply$14$$anonfun$apply$15$$anonfun$apply$16.apply(Directive.scala:162)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRouteResult$1$$anonfun$apply$3.apply(BasicDirectives.scala:59)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRouteResult$1$$anonfun$apply$3.apply(BasicDirectives.scala:59)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRequestContext$1$$anonfun$apply$1.apply(BasicDirectives.scala:41)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRequestContext$1$$anonfun$apply$1.apply(BasicDirectives.scala:41)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRequestContext$1$$anonfun$apply$1.apply(BasicDirectives.scala:41)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRequestContext$1$$anonfun$apply$1.apply(BasicDirectives.scala:41)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:44)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:42)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:44)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:42)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:44)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:42)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:44)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:42)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:44)
        at akka.http.scaladsl.server.RouteConcatenation$RouteWithConcatenation$$anonfun$$tilde$1.apply(RouteConcatenation.scala:42)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRouteResultWith$1$$anonfun$apply$4.apply(BasicDirectives.scala:65)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$mapRouteResultWith$1$$anonfun$apply$4.apply(BasicDirectives.scala:65)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.ExecutionDirectives$$anonfun$handleExceptions$1$$anonfun$apply$1.apply(ExecutionDirectives.scala:32)
        at akka.http.scaladsl.server.directives.ExecutionDirectives$$anonfun$handleExceptions$1$$anonfun$apply$1.apply(ExecutionDirectives.scala:28)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.directives.BasicDirectives$$anonfun$textract$1$$anonfun$apply$6.apply(BasicDirectives.scala:152)
        at akka.http.scaladsl.server.Route$$anonfun$asyncHandler$1.apply(Route.scala:86)
        at akka.http.scaladsl.server.Route$$anonfun$asyncHandler$1.apply(Route.scala:85)
        at akka.stream.impl.fusing.MapAsync$$anon$25.onPush(Ops.scala:1190)
        at akka.stream.impl.fusing.GraphInterpreter.processPush(GraphInterpreter.scala:519)
        at akka.stream.impl.fusing.GraphInterpreter.execute(GraphInterpreter.scala:411)
        at akka.stream.impl.fusing.GraphInterpreterShell.runBatch(ActorGraphInterpreter.scala:588)
        at akka.stream.impl.fusing.GraphInterpreterShell$AsyncInput.execute(ActorGraphInterpreter.scala:472)
        at akka.stream.impl.fusing.GraphInterpreterShell.processEvent(ActorGraphInterpreter.scala:563)
        at akka.stream.impl.fusing.ActorGraphInterpreter.akka$stream$impl$fusing$ActorGraphInterpreter$$processEvent(ActorGraphInterpreter.scala:745)
        at akka.stream.impl.fusing.ActorGraphInterpreter$$anonfun$receive$1.applyOrElse(ActorGraphInterpreter.scala:760)
        at akka.actor.Actor$class.aroundReceive(Actor.scala:517)
        at akka.stream.impl.fusing.ActorGraphInterpreter.aroundReceive(ActorGraphInterpreter.scala:670)
        at akka.actor.ActorCell.receiveMessage(ActorCell.scala:588)
        at akka.actor.ActorCell.invoke(ActorCell.scala:557)
        at akka.dispatch.Mailbox.processMailbox(Mailbox.scala:258)
        at akka.dispatch.Mailbox.run(Mailbox.scala:225)
        at akka.dispatch.Mailbox.exec(Mailbox.scala:235)
        at akka.dispatch.forkjoin.ForkJoinTask.doExec(ForkJoinTask.java:260)
        at akka.dispatch.forkjoin.ForkJoinPool$WorkQueue.runTask(ForkJoinPool.java:1339)
        at akka.dispatch.forkjoin.ForkJoinPool.runWorker(ForkJoinPool.java:1979)
        at akka.dispatch.forkjoin.ForkJoinWorkerThread.run(ForkJoinWorkerThread.java:107)
    Caused by: java.lang.reflect.InvocationTargetException
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
        at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
        at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
        at java.lang.reflect.Constructor.newInstance(Constructor.java:422)
        at org.apache.spark.sql.SparkSession$.org$apache$spark$sql$SparkSession$$reflect(SparkSession.scala:978)
        ... 67 more
    Caused by: org.apache.spark.SparkException: Unable to create database default as failed to create its directory file:/Users/sunlu/Documents/workspace/IDEA/DataMining/spark-warehouse
        at org.apache.spark.sql.catalyst.catalog.InMemoryCatalog.liftedTree1$1(InMemoryCatalog.scala:114)
        at org.apache.spark.sql.catalyst.catalog.InMemoryCatalog.createDatabase(InMemoryCatalog.scala:108)
        at org.apache.spark.sql.internal.SharedState.<init>(SharedState.scala:99)
        at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
        at org.apache.spark.sql.SparkSession$$anonfun$sharedState$1.apply(SparkSession.scala:101)
        at scala.Option.getOrElse(Option.scala:121)
        at org.apache.spark.sql.SparkSession.sharedState$lzycompute(SparkSession.scala:101)
        at org.apache.spark.sql.SparkSession.sharedState(SparkSession.scala:100)
        at org.apache.spark.sql.internal.SessionState.<init>(SessionState.scala:157)
        ... 72 more
    Caused by: java.io.IOException: No FileSystem for scheme: file
        at org.apache.hadoop.fs.FileSystem.getFileSystemClass(FileSystem.java:2584)
        at org.apache.hadoop.fs.FileSystem.createFileSystem(FileSystem.java:2591)
        at org.apache.hadoop.fs.FileSystem.access$200(FileSystem.java:91)
        at org.apache.hadoop.fs.FileSystem$Cache.getInternal(FileSystem.java:2630)
        at org.apache.hadoop.fs.FileSystem$Cache.get(FileSystem.java:2612)
        at org.apache.hadoop.fs.FileSystem.get(FileSystem.java:370)
        at org.apache.hadoop.fs.Path.getFileSystem(Path.java:296)
        at org.apache.spark.sql.catalyst.catalog.InMemoryCatalog.liftedTree1$1(InMemoryCatalog.scala:110)
        ... 80 more

然而使用如下命令执行jar包，则spark初始化成功

`spark-submit target/DataMining-1.0-SNAPSHOT.jar `

> spark-as-service-using-embedded-server <https://github.com/spoddutur/spark-as-service-using-embedded-server>
