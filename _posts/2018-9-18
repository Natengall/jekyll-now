---
layout: post
title: Admin Client API in Kafka Connect
---

Kafka Connect uses the Admin Client API proposed here in KIP-117: https://cwiki.apache.org/confluence/display/KAFKA/KIP-117%3A+Add+a+public+AdminClient+API+for+Kafka+admin+operations

The Admin Client API did not exist until Kafka 0.11.0. Up until this version number, users of Kafka Connect were required to manually create the three internal topics.
But with this changeset in place, users are no longer required to go through this process, and instead, the Admin Client API is used in Kafka Connect to automatically set up the topics required for the Kafka Connect cluster to operate.

Some more details after poking around the code:
Kafka Connect will automatically create a topic with these properties, where every configuration with an asterisk is configurable via worker properties:

Offset
- cleanup policy: log compaction
- *partitions: 25
- *replication factor: 3
https://github.com/apache/kafka/blob/2.0/connect/runtime/src/main/java/org/apache/kafka/connect/storage/KafkaOffsetBackingStore.java#L81-L85

Config
- cleanup policy: log compaction
- partitions: 1
- *replication factor: 3
https://github.com/apache/kafka/blob/2.0/connect/runtime/src/main/java/org/apache/kafka/connect/storage/KafkaConfigBackingStore.java#L424-L428

Status
- cleanup policy: log compaction
- *partitions: 5
- *replication factor: 3
https://github.com/apache/kafka/blob/2.0/connect/runtime/src/main/java/org/apache/kafka/connect/storage/KafkaStatusBackingStore.java#L138-L142

https://github.com/apache/kafka/blob/2.0/connect/runtime/src/main/java/org/apache/kafka/connect/runtime/distributed/DistributedConfig.java#L227-L268

Users provisioning Kafka Connect clusters can always modify the asterisk configs, whereas the other configs are hardcoded into the Kafka Connect source code.
