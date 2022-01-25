1. 查看所有topic

```shell
kafka-topics.sh --list --zookeeper localhost:2181
```

2. 查询topic内容

```shell
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic helloworld --from-beginning
```

3. 创建topic，创建一个叫test的话题，有3个副本，6个partition

```shell
kafka-topics.sh --zookeeper localhost:2181 --create --topic test  --replication-factor 3 --partitions 6
```

4. 删除topic

```shell
kafka-topics.sh --zookeeper localhost:2181 --delete  --topic test
```

5. 查询所有消费者

```shell
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
```



**Note**:以上命令可以直接使用是配置了kafka环境变量，如下：

```shell
export KAFKA_HOME=/data1/kafka
export PATH=$PATH:$KAFKA_HOME/bin
```

