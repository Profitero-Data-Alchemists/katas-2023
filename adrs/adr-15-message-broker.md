# ADR 15: Use self-managed Message Broker

**Date**: 2023-09-15

## Status
Proposed 

## Context
Every event-driven application should have a message broker to transfer data. Given the availability, performance, and scalability requirements, we should consider our choice very carefully.

## Decision
To support an event-driven architecture effectively, employing a scalable message broker is imperative. We recommend using Kafka, a widely recognized and industry-tested software for handling large-scale event streaming. We propose deploying self-hosted open-source Kafka within the Kubernetes environment. This approach helps with cost savings in the initial project phases and aligns with our scalability goals.

## Consequences
 
### Positive
- **High Availability**: Clustered message brokers provide high availability and fault tolerance. In the event of a node failure, the cluster can continue to operate, ensuring that messaging services remain accessible.

- **Scalability**: Clusters allow you to scale your message broker horizontally by adding new nodes. This scalability is important for handling increased message traffic or accommodating growing workloads.

- **Data Durability**: Clustered brokers often offer data durability features, ensuring that messages are stored reliably and can be recovered even after a system failure.

- **Improved Performance**: Clustering can lead to improved message throughput and reduced latency, as multiple nodes can concurrently handle incoming messages and client requests.

### Negative

- **Complexity**: Managing a clustered message broker can be complex. Setting up, configuring, and maintaining the cluster requires expertise and careful planning.

- **Operational Overhead**: Clusters can introduce operational overhead in terms of monitoring, maintenance, and troubleshooting. Ensuring the health and performance of cluster nodes is an ongoing task.

- **Configuration Management**: Managing the configuration of a clustered message broker, including updates and changes, can be challenging to ensure consistency across nodes.