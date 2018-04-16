# Mac下安装使用pyspark

安装py4j 

sudo pip install py4j

vi .bash_profile

export PYTHONPATH=$SPARK_HOME/python/:$PYTHONPATH

source .bash_profile


打开PyCharm，创建一个Project。 然后选择“Run” ->“Edit Configurations” ->“Environment variables” 

增加`SPARK_HOME`目录与`PYTHONPATH`目录。 

- SPARK_HOME:Spark安装目录`/Users/sunlu/Software/spark-2.0.2-bin-hadoop2.6`

- PYTHONPATH:Spark安装目录下的Python目录`/Users/sunlu/Software/spark-2.0.2-bin-hadoop2.6/python`

参考链接

使用PyCharm配置Spark的Python开发环境
<http://blog.tomgou.xyz/shi-yong-pycharmpei-zhi-sparkde-pythonkai-fa-huan-jing.html>


## pyspark自动补全功能

参考链接

<https://blog.csdn.net/u013475704/article/details/78647552>

将`/Users/sunlu/Software/spark-2.0.2-bin-hadoop2.6/python/pyspark`文件夹放入`/Users/sunlu/anaconda/lib/python2.7/site-packages`中。

`/Users/sunlu/anaconda/lib/python2.7/site-packages`为pycharm所使用的python解释器。