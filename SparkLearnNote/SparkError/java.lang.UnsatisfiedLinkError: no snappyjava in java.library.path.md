# java.lang.UnsatisfiedLinkError: no snappyjava in java.library.path

> 参考链接
> 
> 最笨的方法解决 使用Snappy 压缩方式报错“java.lang.UnsatisfiedLinkError: no snappyjava in java.library.path”
> <http://www.itboth.com/d/2uuuAz/snappy-hive-java-linux-spark>
>
> 的UnsatisfiedLinkError：在没有的java.library.path内snappyjava的IntelliJ运行星火MLLib单元测试时，(UnsatisfiedLinkError: no snappyjava in java.library.path when running Spark MLLib Unit test within Intellij)
> <http://www.it1352.com/220649.html>
>
> 使用Snappy 压缩方式报错“java.lang.UnsatisfiedLinkError: no snappyjava in java.library.path”
> <https://blog.csdn.net/stark_summer/article/details/47361603>

- Spark 版本：2.4.3
- Scala版本：2.11.7
- 系统：Mac 10.11.6
- python版本3.6.2
- 时间：2019年8月28日

在intelliJ IDEA中设置 VM options可以暂时解决在IEDA中的报错问题：

在VM options中添加

    -Dorg.xerial.snappy.lib.name=libsnappyjava.jnilib 
    -Dorg.xerial.snappy.tempdir=/tmp
   
但是在pycharm中仍然报错。

修改`spark-defaults.conf`文件，

    vi spark-defaults.conf
   
添加`spark.io.compression.codec lzf`，该方法未能解决问题。

进入`$SPARK_HOME/jars`目录下查看snappy-java JAR包为`snappy-java-1.1.7.3.jar`。

    unzip snappy-java-1.1.7.3.jar
    cd org/xerial/snappy/native/Mac/x86_64/
    cp libsnappyjava.jnilib libsnappyjava.dylib
    cd ../../../../../..
    cp snappy-java-1.1.7.3.jar snappy-java-1.1.7.3.jar.old
    jar cf snappy-java-1.1.7.3.jar org

同时，在环境变量中CLASSPAHT添加`$SPARK_H0ME/jars`

还是没有解决问题。

将`libsnappyjava.dylib`放到`/usr/local/lib/`下，

    cp libsnappyjava.dylib /usr/local/lib/ 
    ls /usr/local/lib/libsnappy* 

问题还是没有解决

最后，将`libsnappyjava.dylib`放到`$JAVA_HOME/jre/lib`下，

    sudo cp libsnappyjava.dylib $JAVA_HOME/jre/lib
    cd $JAVA_HOME/jre/lib
    ls libsnappy*
    
问题解决！！