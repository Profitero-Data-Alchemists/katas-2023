# ADR 6: Use API based synchronous Sharing service

**Date**: 2023-09-15

## Status
Proposed

## Context
Choosing between synchronous and asynchronous communication in a software system is a critical decision that can significantly impact system architecture, performance, scalability, and reliability. The choice between these two communication patterns should be made based on Performance, Scalability, Reliability requirements.

## Decision
We have decided to implement an API-based synchronous sharing service in our application. 
This decision aligns with our need for immediate and real-time sharing of data or resources among users or components within the system. 
By offering synchronous sharing capabilities through APIs, we ensure that users can instantly access and collaborate on shared information, 
enhancing the overall user experience and promoting seamless interactions within our platform.

## Consequences
 
### Positive
- **Real-Time Responsiveness**: Synchronous services provide immediate responses to client requests, making them suitable for scenarios where real-time or near-real-time interaction with the system is essential.

- **Simplicity**: Synchronous APIs are typically simpler to design, implement, and use, as they follow a request-response model that is easy to understand and work with.

- **Predictable Latency**: Synchronous services offer predictable response times, which can be important for applications where response time is critical, such as user interfaces or interactive systems.

### Negative
- **Scalability**: Scaling synchronous services can be challenging, as they may require additional resources to handle concurrent requests. Horizontal scaling may be necessary to meet increased demand.

- **Resource Utilization**: Synchronous services may hold resources (e.g., threads) for the duration of a request, potentially impacting resource utilization and increasing the risk of resource exhaustion.

- **Complex Workflows**: Implementing complex, multi-step workflows with synchronous services can be challenging, as it may involve coordinating multiple synchronous calls.

