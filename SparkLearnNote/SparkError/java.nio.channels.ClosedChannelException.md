# Caused by: java.nio.channels.ClosedChannelException

时间：2017年10月16日
服务：slave6
代码：
spark-submit \
--class com.ecloud.Inglory.RecommendModel.contentModelV2 \
--master yarn \
--deploy-mode client \
--num-executors 4 \
--executor-cores 2 \
--executor-memory 4g \
/root/lulu/Progect/ylzx/RecommendSysV1-1.0-SNAPSHOT-jar-with-dependencies.jar \
yilan-total-analysis_webpage t_hbaseSink ylzx_xgwz ylzx_cnxh

参考链接

spark on yarn提交任务时报ClosedChannelException解决方案 <http://www.genshuixue.com/i-cxy/p/15379851>

Running yarn with spark not working with Java 8 <https://stackoverflow.com/questions/38988941/running-yarn-with-spark-not-working-with-java-8>

解决方案：

修改yarn-site.xml，添加下列property

    <property> 
    <name>yarn.nodemanager.pmem-check-enabled</name> 
    <value>false</value>
    </property>
    <property> 
    <name>yarn.nodemanager.vmem-check-enabled</name> 
    <value>false</value>
    </property>

分析：

按照上述配置提供的信息，目测是我给节点分配的内存太小，yarn直接kill掉了进程，导致ClosedChannelException

 