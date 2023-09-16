# ADR 10: Use same Partitioning Key for corresponding topics and tables

**Date**: 2023-09-15

## Status
Proposed 

## Context
Choosing the correct partitioning key is crucial when designing a database / topic schema for a distributed system, such as those used in NoSQL databases or Message Brokers. The partitioning key determines how data is distributed across nodes or partitions in the database, and it has a significant impact on performance, scalability, and data retrieval efficiency. 

## Decision
In this architecture, we assume that the partitioning key for the topic and table will be identical. This alignment enables the update microservice to transfer data without interruptions from other microservices. Consequently, we ensure that all updates for the same record are sequentially placed in the same partition, preventing collisions during updates.


## Consequences
 
### Positive
- **Scalability**: A partitioning key enables horizontal scalability by distributing data across multiple nodes or partitions. This allows you to handle increased data volume and traffic by adding more nodes or partitions as needed.

- **Performance**: Properly chosen partitioning keys can lead to improved read and write performance. Queries that involve the partitioning key can be executed on a single partition, resulting in low latency and efficient data retrieval. Write operations can be distributed evenly across partitions, reducing contention.

- **Load Balancing**: An effective partitioning key helps evenly distribute data across partitions or nodes, preventing hotspots where some partitions are overloaded with data while others are underutilized. This load balancing ensures consistent performance and resource utilization.

### Negative
- **Data Distribution Changes**: As data patterns change over time, the chosen partitioning key may become suboptimal. Adapting the partitioning key to accommodate changing data distribution patterns is essential but can be complex.

- **Data Integrity**: Choosing the wrong partitioning key can lead to data fragmentation, where related data is spread across multiple partitions. This can make it challenging to maintain data integrity and consistency.

- **Complexity**: Managing a partitioned data system, especially one with a complex partitioning strategy, can be challenging. It may require additional tooling and expertise.

