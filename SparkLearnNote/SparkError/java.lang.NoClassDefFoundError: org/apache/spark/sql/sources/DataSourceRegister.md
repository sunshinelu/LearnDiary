# java.lang.NoClassDefFoundError: org/apache/spark/sql/sources/DataSourceRegister

因为一系列的神操作，我也不知道我干了啥。然后，悲剧了.....

报错信息如下：
    
    
    Traceback (most recent call last):
      File "/Users/sunlu/Workspaces/PyCharm/PythonDiary/Spark/DataBase/ElasticSearch/pyspark_es_demo1.py", line 53, in <module>
        .mode('append') \
      File "/anaconda3/lib/python3.7/site-packages/pyspark/sql/readwriter.py", line 732, in save
        self._jwrite.save()
      File "/anaconda3/lib/python3.7/site-packages/py4j/java_gateway.py", line 1257, in __call__
        answer, self.gateway_client, self.target_id, self.name)
      File "/anaconda3/lib/python3.7/site-packages/pyspark/sql/utils.py", line 63, in deco
        return f(*a, **kw)
      File "/anaconda3/lib/python3.7/site-packages/py4j/protocol.py", line 328, in get_return_value
        format(target_id, ".", name), value)
    py4j.protocol.Py4JJavaError: An error occurred while calling o46.save.
    : java.lang.NoClassDefFoundError: org/apache/spark/sql/sources/DataSourceRegister
        at java.lang.ClassLoader.defineClass1(Native Method)
        at java.lang.ClassLoader.defineClass(ClassLoader.java:760)
        at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
        at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
        at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:411)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:411)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        at java.lang.Class.forName0(Native Method)
        at java.lang.Class.forName(Class.java:348)
        at java.util.ServiceLoader$LazyIterator.nextService(ServiceLoader.java:370)
        at java.util.ServiceLoader$LazyIterator.next(ServiceLoader.java:404)
        at java.util.ServiceLoader$1.next(ServiceLoader.java:480)
        at scala.collection.convert.Wrappers$JIteratorWrapper.next(Wrappers.scala:43)
        at scala.collection.Iterator$class.foreach(Iterator.scala:891)
        at scala.collection.AbstractIterator.foreach(Iterator.scala:1334)
        at scala.collection.IterableLike$class.foreach(IterableLike.scala:72)
        at scala.collection.AbstractIterable.foreach(Iterable.scala:54)
        at scala.collection.TraversableLike$class.filterImpl(TraversableLike.scala:247)
        at scala.collection.TraversableLike$class.filter(TraversableLike.scala:259)
        at scala.collection.AbstractTraversable.filter(Traversable.scala:104)
        at org.apache.spark.sql.execution.datasources.DataSource$.lookupDataSource(DataSource.scala:630)
        at org.apache.spark.sql.DataFrameWriter.save(DataFrameWriter.scala:245)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:497)
        at py4j.reflection.MethodInvoker.invoke(MethodInvoker.java:244)
        at py4j.reflection.ReflectionEngine.invoke(ReflectionEngine.java:357)
        at py4j.Gateway.invoke(Gateway.java:282)
        at py4j.commands.AbstractCommand.invokeMethod(AbstractCommand.java:132)
        at py4j.commands.CallCommand.execute(CallCommand.java:79)
        at py4j.GatewayConnection.run(GatewayConnection.java:238)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.ClassNotFoundException: org.apache.spark.sql.sources.DataSourceRegister
        at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        ... 40 more


修复macOS 10.12 的 Python2.7环境(为了让Xcode能打包)
https://www.jianshu.com/p/2aa2b973d025

【python】mac系统下的Python安装，以及误删系统Python的解决办法
https://blog.csdn.net/weifenglin1997/article/details/61419689


对python卸载安装，安装卸载。

安装anaconda python37，仍然报错，卸载python37。
安装anaconda python36，还是报错，卸载python36。


使用`brew install python`安装python，但是安装的是python3.7，然后又是一轮卸载`brew uninstall --force python`。

然后用pyenv进行安装python3.6。

    brew update 
 
    brew install pyenv 
    # 查看能够安装的版本：
    pyenv install --list 
    # 只有版本号的为官方的版本，其他的为衍生版。
    pyenv install 3.6.2 -v
    
    pyenv rehash

    pyenv versions 
    
    pyenv global 3.6.2
     
    pyenv versions 
    
    export PATH="$HOME/.pyenv/bin:$PATH"
    eval "$(pyenv init -)"
    eval "$(pyenv virtualenv-init -)"
    
    python

配置环境变量：vi .bash_profile

    export PATH=$PATH:$HOME/.pyenv/bin
    eval "$(pyenv init -)"

如果卸载的话，使用命令：（目前尚未卸载）

    pyenv uninstall 3.6.2
    python
    
    
#### 结论

终于找到问题的原因，在使用spark读取ES数据的时候，将`elasticsearch-spark-20_2.11-6.2.3.jar`放到了`$JAVA_HOME/jre/lib/ext`目录下面，
导致在加载java环境的时候默认读取这个路径下的java包方法，才会出现`java.lang.NoClassDefFoundError: org/apache/spark/sql/sources/DataSourceRegister`的报错。