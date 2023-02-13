  https://www.interviewbit.com/kafka-interview-questions/
  - Producer
    - Topic
    - number of Partition
    -  replication factor
    -  key serializer
    -  value serializer
  -  Consumer
     - Topic
     - consumer groupid
     - offset
      - earlier
      - latest
      - random
     - key de-serializer
     - value de-serializer
  -  Broker
     - Partition
     - Leader
     - Insync node


1. If no partition stratergy then send message to kafka using the partition key as null and then message will come in all partitions so that different consumer group can use different partition and offset and scale
2. If one broker goes down the other broker will provide the fault tolerance
3. Zoo keeper have limitations on kafka scalability hence kraft protocol so that all broker have metadata of other brokers and instead of relying on the zookeeper can query the boot-strap broker to get information that zookeeper gives
4. What is poison pill in Kafka?
   During de-serialization if any message fails then the offset is not incremented and hence kafka will not give the next message, so in the callback handler consumer record object have means to update the ack and we can put the message to dead letter queue or simply skip it
5. JMS listener is push based and kafka listener is pull based
