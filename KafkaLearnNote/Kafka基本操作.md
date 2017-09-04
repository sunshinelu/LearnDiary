# Kafka基本操作

参考链接：
Kafka Shell基本命令（包括topic的增删改查）
http://www.cnblogs.com/xiaodf/p/6093261.html#2

Kafka安装位置：

"bigdata1.yiyun:2181,bigdata2.yiyun:2181,bigdata4.yiyun:2181,bigdata5.yiyun:2181,slave3.yiyun:2181"

查看当前topic的主题数：

kafka-topics.sh --zookeeper bigdata4:2181 --list

查看日志

/usr/soft/kafka/logs
vi server.log