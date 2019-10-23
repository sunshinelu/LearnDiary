# Kafka error value must be bytes.md



```python
from kafka import KafkaProducer
from kafka import KafkaConsumer
from kafka.errors import KafkaError
import time


def main():
    ##生产模块
    producer = KafkaProducer(bootstrap_servers=['127.0.0.1:9092'])
    with open("/Users/sunlu/Workspaces/PyCharm/Evay-PySpark-Demo/data/json.txt", 'r') as f:
        for line in f.readlines():
            time.sleep(1)
            producer.send("test",line)
            print
            line
            # producer.flush()


if __name__ == '__main__':
    main()
```
报错 `value must be bytes`。

解决方案：对内容进行类型转换 将 `line` 改为 `bytes(line, 'utf-8')`。

```python
from kafka import KafkaProducer
from kafka import KafkaConsumer
from kafka.errors import KafkaError
import time


def main():
    ##生产模块
    producer = KafkaProducer(bootstrap_servers=['127.0.0.1:9092'])
    with open("/Users/sunlu/Workspaces/PyCharm/Evay-PySpark-Demo/data/json.txt", 'r') as f:
        for line in f.readlines():
            time.sleep(1)
            producer.send("test", bytes(line, 'utf-8'))
            print
            line
            # producer.flush()


if __name__ == '__main__':
    main()
```