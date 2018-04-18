# Jupyter中使用PySpark

> 参考链接：
> Anaconda中配置Pyspark的Spark开发环境
> <https://www.cnblogs.com/jackchen-Net/p/6667205.html>
>
> Get Started with PySpark and Jupyter Notebook in 3 Minutes
> <https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f>



## Jupyter中使用PySpark Mac版

### 方法一：使用配置环境变量的方法

安装Jupyter notebook:

    pip install jupyter

vi .bash_profile

export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/jars

source .bash_profile

### 方法二：使用findspark包

安装findspark

    pip install findspark

启动jupyter

    jupyter notebook

输入以下命令：

    import findspark
    findspark.init()
    import pyspark
    import random
    sc = pyspark.SparkContext(appName="Pi")
    num_samples = 100000000
    def inside(p):     
      x, y = random.random(), random.random()
      return x*x + y*y < 1
    count = sc.parallelize(range(0, num_samples)).filter(inside).count()
    pi = 4 * count / num_samples
    print(pi)
    sc.stop()




## Jupyter中使用PySpark windows版

1. 修改环境变量：

PYSPARK_DRIVER_PYTHON  ipython
PYSPARK_DRIVER_PYTHON_OPTS  notebook
PYTHONPATH %SPARK_HOME%\python\lib\py4j;%SPARK_HOME%\python\lib\pyspark

2. 修改％SPARK_HOME%\conf下的spark-env文件

export PYSPARK_PYTHON=/D:/ProgramFiles/Anaconda3
export PYSPARK_DRIVER_PYTHON=/D:/ProgramFiles/Anaconda3
export PYSPARK_SUBMIT_ARGS='--master local[*]'


3. 将%SPARK_HOME%\ppython\pyspark 文件夹copy到D:/ProgramFiles/Anaconda3/Lib/site-packages 目录下