# 在Win10下安装Spark

Spark版本：Spark 2.2.0

Python版本：Python 3.6.1

注意：Spark2.1.0以上才支持Python3.6！

## 1. 下载Spark和Hadoop

1) 在http://spark.apache.org/downloads.html中下载`spark-2.2.0-bin-hadoop2.6.tgz`，并解压到`D:\ProgramFiles\Apache`中。

2） 下载`winutils.exe`，将`winutils.exe`放在`C:\hadoop\bin`中。

## 2. 配置Spark和Hadoop

1） 此电脑 > 点击右键 > 属性 > 高级系统设置 > 环境变量 > 系统环境变量：

(1) 配置`HADOOP_HOME`和`SPARK_HOME`，`HADOOP_HOME`为`C:\hadoop`，`SPARK_HOME`为`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6`。

(2) 将`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6\bin`、`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6\jars`、`C:\hadoop\bin`添加到`Path`中。

注意：新添加的环境变量要置顶。

2) 设置`HADOOP_HOME`，并将winutils.exe放置在`HADOOP_HOME\bin`目录下后，使用管理员权限执行如下命令：


    C:\hadoop\bin\winutils.exe chmod 777 C:\tmp\hive

查看`C:\hadoop\bin\winutils.exe ls -F C:\tmp\hive`运行情况，则输入以下命令：

     C:\hadoop\bin\winutils.exe ls C:\tmp\hive
drwxrwxrwx 1 SUNLU\sunlu SUNLU\None 0 Jul 21 2017 C:\tmp\hive


参考链接：
https://stackoverflow.com/questions/34196302/the-root-scratch-dir-tmp-hive-on-hdfs-should-be-writable-current-permissions

## 3 在PyCharm中配置Spark

### 1).安装PyCharm和py4j

    sudo pip install py4j


### 2). 配置PyCharm

打开PyCharm，创建一个Project。 然后选择“Run” ->“Edit Configurations” ->“Environment variables” 

增加`SPARK_HOME`目录与`PYTHONPATH`目录。 

- SPARK_HOME:Spark安装目录`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6`

- PYTHONPATH:Spark安装目录下的Python目录`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6\python`


使用PyCharm配置Spark的Python开发环境，参考链接：

http://blog.tomgou.xyz/shi-yong-pycharmpei-zhi-sparkde-pythonkai-fa-huan-jing.html

## 4 运行过程中出现的错误：


### 1) 在PyCharm中报错：java.io.IOException: Could not locate executable D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6\bin\winutils.exe in the Hadoop binaries.

将`winutils.exe`放入`D:\ProgramFiles\Apache\spark-2.2.0-bin-hadoop2.6\bin`目录下即可。

### 2) 在PyCharm中报错：TypeError: 'str' object is not callable
Traceback (most recent call last):
  File "D:/Workspace/PyCharm/pythonDemo/SparkDemo/SparkDemo3.py", line 14, in <module>
    spark.version()
TypeError: 'str' object is not callable

    spark.version()

修改为：

    spark.version
    
运行正常！

参考链接：
https://stackoverflow.com/questions/23691532/python-3-4-typeerror-str-object-is-not-callable


### 3) 在PyCharm中报错：TypeError: 'Builder' object is not callable

Traceback (most recent call last):
  File "D:/Workspace/PyCharm/pythonDemo/SparkDemo/SparkDemo3.py", line 11, in <module>
    spark = SparkSession.builder()\
TypeError: 'Builder' object is not callable

    spark = SparkSession.builder()\
        .appName("SparkDemo3")\
        .getOrCreate()
        
修改为：

    spark = SparkSession.builder\
    .appName("SparkDemo3")\
    .getOrCreate()
    
后，运行正常！

参考链接：
https://stackoverflow.com/questions/41353522/typeerror-builder-object-is-not-callable-spark-structured-streaming


### 4) spark-shell报错：在退出spark-shell的时候出现如下错误：ERROR ShutdownHookManager: Exception while deleting Spark temp dir: C:\Users\sunlu\AppData\Local\Temp\spark-de9c702e-0216-4029-b34b-20e14b2a63b0

【未解决】
目前spark在windows系统下存在这个问题。不想看的话，就把log4j.properties中log的level设置为FATAL吧。
【注意】在Spark使用结束时，务必使用 :quit 退出。否则将导致错误。