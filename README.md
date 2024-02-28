# Kafka

### Step 1: Download the [KAFKA](https://www.apache.org/dyn/closer.cgi?path=/kafka/3.7.0/kafka_2.13-3.7.0.tgz), which is basically used to start 
- Start Zookeeper
- Start Kafka server
- Create Kafka topics

`$ tar -xzf kafka_2.13-3.7.0.tgz`

`$ cd kafka_2.13-3.7.0`

### Step 2: Start Zookeeper
`$ bin/zookeeper-server-start.sh config/zookeeper.properties`

![Zookeeper](./images/1.png)
### Step 3: Start Kafka server
`$ bin/kafka-server-start.sh config/server.properties`

![Kafka](./images/2.png)

### Step 4: Create Kafka topics
Create topic "streams-plaintext-input". Used to publish events which will act as input to the WordCountDemo class

```
$ bin/kafka-topics.sh --create \
--bootstrap-server localhost:9092 \
--replication-factor 1 \
--partitions 1 \
--topic streams-plaintext-input 
```

Create topic "streams-wordcount-output" WordCountDemo will process "streams-plaintext-input" events and emit events to "streams-wordcount-output".

```
$ bin/kafka-topics.sh --create \
--bootstrap-server localhost:9092 \
--replication-factor 1 \
--partitions 1 \
--topic streams-wordcount-output \
--config cleanup.policy=compact
```

Check if both the topic got created

`$ bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe`

![Topics](./images/3.png)

### Step 5: Start the Wordcount Application
Run WordCountDemo class using intellij

### Step 6: Start Kafka producer which will emit events on topic streams-plaintext-input

```
$ bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic streams-plaintext-input
```

![Topics](./images/4.png)

### Step 7: Start Kafka consumer which will consume events produced by Wordcount Application

```
$ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
--topic streams-wordcount-output \
--from-beginning \
--formatter kafka.tools.DefaultMessageFormatter \
--property print.key=true \
--property print.value=true \
--property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
--property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
![Topics](./images/5.png)
![Topics](./images/6.png)

