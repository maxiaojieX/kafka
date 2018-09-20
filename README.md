
# kafka单节点安装测试

<hr>
<br>
一、下载kafka，这里kafka版本为kafka_2.12<br>
二、kafka压缩包中带有zookeeper，编辑/config/zookeeper.properties文件，启动zookeeper<br>
bin/zookeeper-server-start.sh config/zookeeper.properties &

```
# the directory where the snapshot is stored.
dataDir=/tmp/zookeeper
# the port at which the clients will connect
clientPort=2181
```
<br>
三、配置server.properties文件，启动kafka broker<br>
bin/kafka-server-start.sh config/server.properties &

```
 # The id of the broker. This must be set to a unique integer for each broker.
broker.id=0

# The port the socket server listens on
port=9092

# A comma seperated list of directories under which to store log files
log.dirs=/tmp/kafka-logs

# Zookeeper connection string (see zookeeper docs for details).
# This is a comma separated host:port pairs, each corresponding to a zk
# server. e.g. "127.0.0.1:3000,127.0.0.1:3001,127.0.0.1:3002".
# You can also append an optional chroot string to the urls to specify the
# root directory for all kafka znodes.
zookeeper.connect=localhost:2181
```
<br>
四、创建topic<br>
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
<br><br>
五、配置producer.properties文件，启动生产者 生产消息<br>

bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test<br>
\>this is a message
<br><br>
六、配置consumer.properties文件，消费消息<br>
注意:<br>
0.90版本之前启动消费者:bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
<br>
0.90版本之后启动消费者:bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
<br><br>
七、错误提示
```
Number of alive brokers '1' does not meet the required replication factor '3' for the offsets topic (configured via 'offsets.topic.replication.factor'). This error can be ignored if the cluster is starting up and not all brokers are up yet. (kafka.server.KafkaApis)
```
解决办法:
offsets.topic.replication.factor=1<br>

server.properties中缺少上诉属性，如果缺少上述属性，则默认值为3。






