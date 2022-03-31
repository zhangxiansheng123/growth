* Kafka tools使用遇到的坑一直连接不上

```shell
# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
#   这里是自己主机ip
listeners=PLAINTEXT://0.0.0.0:9092

# Hostname and port the broker will advertise to producers and consumers. If not set, 
# it uses the value for "listeners" if configured.  Otherwise, it will use the value
# returned from java.net.InetAddress.getCanonicalHostName().
# 这里是对应的外网ip,只有内网的话必须把改内网对应端口映射出去，并使用外网ip及映射端口
advertised.listeners=PLAINTEXT://47.116.75.99:9092
```

* kafka-ui-lite 可以看kafka集群、发送消息、消费消息，但是不能看见topic消息，可以看zookeeper节点等功能
* kafka-manager 可以重新分配partitation、查看kafka集群、查看topic等功能
* [kafka-map](https://zhuanlan.zhihu.com/p/380301924)还是挺强大的

```shell
docker run -d -p 8080:8080 -v /app/deploy/kafka-map/data:/usr/local/kafka-map/data -e DEFAULT_USERNAME=admin -e DEFAULT_PASSWORD=admin --name kafka-map --restart always dushixiang/kafka-map:latest

# 连不上的话hosts加上ip
docker exec kafka-map  bash -c "echo 172.21.125.244  node-app-1    node-app-1 172.21.190.149  node-data-1   node-data-1 172.21.190.152  node-data-2   node-data-2 172.21.190.150  node-data-3   node-data-3 >> /etc/hosts"

# 或者进入容器里加ip
docker exec -it kafka-map bash
# 容器里面没有vi、vim只能使用cat和EOF组合
cat <<EOF > /etc/hosts
172.21.125.244  node-app-1    node-app-1
172.21.190.149  node-data-1   node-data-1
172.21.190.152  node-data-2   node-data-2
172.21.190.150  node-data-3   node-data-3
EOF
```





