# Apache Kafka
#article #tech #kafka

> A distributed streaming platform  

_Streams are just infinite data, data that never ends. It just keep arriving and you can process it in real time_

_Distributed means that Kafka works in a cluster, each node in the cluster is called Broker. Brokers are servers executing a copy of apache Kafka_

Kafka is a set of machines working together to be able to handle and process real-time infinite data.

* Resilient 
* Reliable
* Scalable
* Fault-tolerant
- - - -

## Message Queue Concept

![](Apache%20Kafka/MessageQueueConcept.drawio.png)

## Kafka Architecture
![](Apache%20Kafka/kafka-broker-beginner.png)

As it is very similar to Message Queue Concept, Kafka is often misunderstood that it’s potential is only limited as _just another messaging system_.

* In Kafka, do not possess a **Queue** but instead having the concept of **Topics**

### Topics & Partitions
Topics is a very specific type of data stream whereby;
* A topic is divided into **partitions**, each topic can have one or more partitions and we are required to specify the number of partitions when creating the topic.
* If we do not give any key to a message while producing it, by default, the producers will send the message in a **round-robin** way, each partition will receive a message (even if they are the sent by the same producer).
Because of that, we are not able to guarantee the delivery order at the partition level.
* If we would like to sent a message always to the same partition, we need to give a **key** to our messages, to ensure the message will be sent to the same partition.
* _Each message will be stored in the broker disk, and will receive an **offset**. This offset is unique at the partition level, each partition has its owns offset._
* The consumers uses the offset to read the messages, they read from the oldest to the newest. In case of consumer failure, when it recovers, it will start reading from the last offset.

![](Apache%20Kafka/1*9Qm9qjZbvfV0X1pAlUUxcw.png)

/Another Source - Understanding Kafka Topic Partitions medium.com/

Kafka’s topics are divided into several partitions. While the topic is a logical concept in Kafka, **a partition is the smallest storage unit that holds a subset of records owned by a topic** 
Each partition is a single log file where records are written to it in an append-only fashion.

![](Apache%20Kafka/A9CED8F8-3105-4CB0-A5F3-B1AB5262AF3E.png)

### Offset and ordering of the messages
_The records in the partitions are each assigned a sequential identifier called the offset, which is unique for each record within the partition._

The offset is an incremental and immutable number, maintained by Kafka. When a record is written to a partition, it is appended to the end of the log, assigning the next sequential offset. 

The figure shows a topic with three partitions, and records are being appended to the end of each one.


![](Apache%20Kafka/9D0E4B55-78F5-4526-9E42-C8F7E746E021.png)

Although the messages within a partition are ordered, messages across a topic are not guaranteed to be ordered.

### Partitions are the way Kafka provides scalability

A Kafka cluster is made of one or more servers. In the Kafka architecture, they are called Brokers. Broker holds a subset of records that belongs to the entire cluster.

Kafka distributes the partitions of a particular topic across multiple brokers. By doing so;

* If are to put all the partitions of a topic in a single broker, the scalability of that topic is constrained by the broker’s IO throughput. A topic will never get bigger than the biggest machine in the cluster. Spreading partitions across multiple brokers, a single topic can be scaled horizontally to provide performance far beyond a single broker’s ability.
* A single topic can be consumed by multiple consumers in parallel. Serving all partitions form a single broker limits the number of consumers it can support. Partitions on multiple brokers enable more consumers.
* Multiple instances of same consumer can connect to partitions on different brokers, allowing very high message processing throughput. Each consumer instance will be served by one partition, ensuring that each record has a clear processing owner.

### Writing Records to partitions

### Using partition key

A producer may use a partition key to direct messages to a specific partition. Ap partition key can be any value that can be derived from the application context. A unique device ID / user ID will make a good partition key.

By default, the partition key is passed through a hashing function, which creates the partition assignment. That assures that all records produced with the same key, will arrive at the same partition.

 Specifying a partition key enables keeping related events together in the same partition and in the exact order in which they were sent.

![](Apache%20Kafka/97B82465-4A4A-4050-9FE3-4BBED881A585.png)

Key based partition assignment can lead to a broker skew if keys are not distributed fairly.

Eg. When a customer ID is used as the partition key, one customer could generate 90% of the traffic, and only one partition will receive 90% of the traffic. This may cause broker failure.

### Allowing Kafka to decide the partition

By default, Kafka uses the round-robin partition assignment. Those records will be written evenly across all partitions of a particular topic. Ordering of records cannot be guaranteed in such of the case.

### Writing a custom partitioner

A producer can use its own partitioner implementation that uses other business rules to do the partition assignment.

### Reading records from partitions

Consumers pull messages off Kafka topic partitions, by connecting to a partition in a broker, reading the messages in the order that they were written.

The offset of a message works as a consumer side cursor. The consumer keeps track of which messages it has already consumed by keeping track of the offset of messages. After reading a message, the consumer advances its cursor to the next offset in the partition and continues. Advancing and remembering the last read offset within a partition is the responsibility of the consumer.

By remembering the offset of the last consumed message for each partition, a consumer can join a partition at the point in time they choose and resume form there. This is useful for consumer to resume reading after recovering from a crash.


![](Apache%20Kafka/64A71DFF-5694-4E75-A215-854CE4719EBA.png)


A partition can be consumed by one or more consumers, each reading at different offsets.
Kafka has the concept of **consumer groups** where several consumers are grouped to consume a given topic. Consumers in the same consumer group are assigned the same **group-id** value.
The consumer group concept ensures that message is only ever **READ BY A SINGLE CONSUMER IN THE GROUP**

> When a consumer group consumes the partitions of a topic, Kafka makes sure that each partition is consumed by exactly one consumer in the group.  

