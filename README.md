# kafka-streams
This is a Kafka Streams application, where the input and output data are stored in an Apache Kafka® cluster.

## Getting Started (Mac)
1. Install Kafka with Homebrew

```
brew install kafka
```

2. Start zookeeper

```bash
zookeeper-server-start /opt/homebrew/etc/kafka/zookeeper.properties

```

3. Start Kafka

```bash
kafka-server-start /opt/homebrew/etc/kafka/server.properties
```

## Favourite-Colour

### This module 
- take a comma delimited topic of userid,color
- get running count of favourite colours overall and output to a topic

### Running locally
1. create input topic
```
kafka-topics --bootstrap-server 127.0.0.1:9092 --create --topic favourite-colour-input --partitions 1 --replication-factor 1
```
2. create intermediary log compacted topic
```
kafka-topics --bootstrap-server 127.0.0.1:9092 --create --topic user-keys-and-colours --partitions 1 --replication-factor 1 --config cleanup.policy=compact
```
3. create output log compacted topic
```
kafka-topics --bootstrap-server 127.0.0.1:9092 --create --topic favourite-colour-output --partitions 1 --replication-factor 1 --config cleanup.policy=compact
```
4. launch a Kafka consumer
```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
    --topic favourite-colour-output \
    --from-beginning \
    --formatter kafka.tools.DefaultMessageFormatter \
    --property print.key=true \
    --property print.value=true \
    --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
    --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
5. launch the streams application
6. produce data to it
```
kafka-console-producer --bootstrap-server localhost:9092 --topic favourite-colour-input
```
