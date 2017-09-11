# Kafka基本操作

参考链接：
Kafka Shell基本命令（包括topic的增删改查）
http://www.cnblogs.com/xiaodf/p/6093261.html#2

Kafka安装位置：

"bigdata1.yiyun:2181,bigdata2.yiyun:2181,bigdata4.yiyun:2181,bigdata5.yiyun:2181,slave3.yiyun:2181"

查看当前topic的主题数：

kafka-topics.sh --zookeeper bigdata4:2181 --list

lqzTest
ylzx

查看日志

/usr/soft/kafka/logs
vi server.log

查看指定topic信息
kafka-topics.sh --zookeeper bigdata4:2181 --describe --topic ylzx

Topic:ylzx	PartitionCount:3	ReplicationFactor:3	Configs:
	Topic: ylzx	Partition: 0	Leader: 2	Replicas: 2,0,1	Isr: 2,0,1
	Topic: ylzx	Partition: 1	Leader: 2	Replicas: 0,1,2	Isr: 2,1
	Topic: ylzx	Partition: 2	Leader: 1	Replicas: 1,2,0	Isr: 2,1,0