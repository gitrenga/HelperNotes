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
4. 
