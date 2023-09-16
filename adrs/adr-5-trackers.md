# ADR 5: Use messaging based asynchronous Trackers

**Date**: 2023-09-15

## Status
Proposed

## Context
Choosing between synchronous and asynchronous communication in a software system is a critical decision that can significantly impact system architecture, performance, scalability, and reliability. The choice between these two communication patterns should be made based on Performance, Scalability, Reliability requirements.

## Decision
We made the decision for our trackers (emails and reservations) to adopt a messaging-based asynchronous service approach. 
This choice aligns with our goal of enhancing system performance, scalability, and fault tolerance by decoupling trackers from external APIs, allowing to adapt to performance of Email or Travel Agency/System APIs. 
Asynchronous tracking will enable us to optimize our processing capabilities and deliver a more resilient and efficient system overall.

## Consequences
 
### Positive
- **Scalability**: Asynchronous messaging can facilitate scalability. Multiple subscribers can process messages concurrently, making it easier to handle increased message volumes or growing workloads.

- **Performance**: Asynchronous processing can improve overall system performance. Publishers can offload time-consuming or resource-intensive tasks to subscribers, optimizing resource utilization.

- **Reliability**: Asynchronous messaging systems often include mechanisms for message durability and reliability. Messages can be safely stored and retried in case of failures, reducing the risk of data loss.

### Negative
- **Complexity**: Asynchronous messaging can introduce complexity into system design and implementation. Managing message queues, retries, and asynchronous communication patterns requires careful planning.

- **Concurrency Issues**: Handling concurrent message processing in subscribers may require additional synchronization mechanisms to ensure data consistency and prevent race conditions.

- **Error Handling**: Handling errors and exceptions in asynchronous systems can be complex, as errors may not be immediately visible to the sender. Robust error handling and retry mechanisms are essential.

### Risks
- TBD