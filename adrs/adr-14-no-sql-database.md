# ADR 14: Use NoSQL Database with object-oriented model

**Date**: 2023-09-15

## Status
Proposed 

## Context
Every application should have a database to persist data. Given the availability, performance, and scalability requirenments, we should consider our choice very carefully.

## Decision
To achieve a highly scalable and available system while keeping performance at exceptional level, opting for a NoSQL clustered database is essential. We recommend using self-managed MongoDB for reasons similar to the Message Broker's. Self-management within Kubernetes enables us to control costs while ensuring robust database performance and availability.

## Consequences
 
### Positive
- **Flexibility and Schema-less Design**: NoSQL databases are schema-less, which means you can store data without defining a rigid schema in advance. This flexibility is particularly useful when dealing with unstructured or semi-structured data.

- **Scalability**: NoSQL databases are designed to handle large volumes of data and high traffic loads. They can scale horizontally, adding more servers or nodes to accommodate increased data and traffic.

- **High Performance**: Many NoSQL databases are optimized for read and write operations, making them suitable for applications that require low latency and high throughput, such as real-time analytics and gaming.

- **Simplified Replication and Sharding**: NoSQL databases often provide built-in support for data replication and sharding, making it easier to ensure data availability and distribute data across multiple servers or data centers.

### Negative

- **Lack of ACID Transactions**: Most NoSQL databases sacrifice strong ACID (Atomicity, Consistency, Isolation, Durability) transactions for performance and scalability. This can lead to data consistency issues in certain scenarios.

- **Limited Query Capabilities**: NoSQL databases typically lack the advanced querying features of relational databases, making it challenging to perform complex joins and queries.

- **Data Integrity Challenges**: With a flexible schema, maintaining data integrity and consistency may require more effort in NoSQL databases compared to traditional relational databases.

