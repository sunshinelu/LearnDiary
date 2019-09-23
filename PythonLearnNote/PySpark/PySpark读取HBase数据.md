# PySpark读取HBase数据

1. PySpark基于SHC框架读取HBase数据并转成DataFrame
<https://blog.csdn.net/u014736152/article/details/89605015>


2. Spark2.1.0+入门：读写HBase数据(Python版)
<http://dblab.xmu.edu.cn/blog/1715-2/>


<https://mvnrepository.com/artifact/org.apache.spark/spark-examples_2.11/1.6.0-typesafe-001>

bigdata7:

hbase shell

list

create 't_student_sunlu','info'

describe 't_student_sunlu'

//首先录入student表的第一个学生记录
put 't_student_sunlu','1','info:name','Xueqian'
put 't_student_sunlu','1','info:gender','F'
put 't_student_sunlu','1','info:age','23'
//然后录入student表的第二个学生记录
put 't_student_sunlu','2','info:name','Weiliang'
put 't_student_sunlu','2','info:gender','M'
put 't_student_sunlu','2','info:age','24'

//如果每次只查看一行，就用下面命令
get 't_student_sunlu','1'
//如果每次查看全部数据，就用下面命令
scan 't_student_sunlu'

count 't_student_sunlu'


bigdata7:

/usr/hdp/2.2.4.2-2/hbase/lib

cp hbase*.jar /root/lulu/jars/hbase/
cp guava-12.0.1.jar /root/lulu/jars/hbase/
cp htrace-core-3.0.4.jar /root/lulu/jars/hbase/
cp protobuf-java-2.5.0.jar /root/lulu/jars/hbase/
