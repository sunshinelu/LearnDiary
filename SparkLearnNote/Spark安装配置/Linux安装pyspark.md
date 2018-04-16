# Linux安装pyspark


vi .bashrc


# add spark environment - by sunlu - start
SCALA_HOME=/home/gpuserver/softwares/scala-2.11.8
SPARK_HOME=/home/gpuserver/softwares/spark-2.1.0-bin-hadoop2.6
export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/jars:$SCALA_HOME/jars:$SCALA_HOME/bin
# add spark environment - by sunlu - end

source .bashrc

将`$SPARK_HOME/python/pyspark/`文件夹复制到`/home/gpuserver/softwares/spark-2.1.0-bin-hadoop2.6/python/pyspark/`中。


启动spark-shell

spark-shell 

启动 pyspark 
pyspark

启动python

python

import pyspark