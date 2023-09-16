# ADR 8: Use separate Scaling Groups for different workloads

**Date**: 2023-09-15

## Status
Proposed 

## Context
Services in a distributed system should be designed to scale differently based on their specific characteristics, workload, and requirements. Scaling each service independently allows for efficient resource allocation and ensures that system performance, reliability, and cost-effectiveness are optimized. 

## Decision
Three scaling groups was defined:

#### API Scaling Group:

- This group is responsible for handling user requests, and the call triggers are the system users.
- It ensures efficient processing of incoming requests and can scale horizontally to meet varying user demand.

#### Data Readers/Updaters Scaling Group:

- Dedicated to reading and updating data entities, this group responds to tasks initiated by other actors.
- It facilitates data retrieval and manipulation efficiently and can adjust its capacity based on workload.

#### Messaging Scaling Group:

- This group specializes in processing tasks within a queue of messages/tasks.
- Its members operate independently, executing tasks from a predefined task list.

Scaling ensures that the messaging workload is managed effectively.
These scaling groups optimize resource allocation and task handling across the system, enhancing performance and responsiveness.


## Consequences
 
### Positive
- **Resource Efficiency**: Scaling services differently allows you to allocate resources based on their actual needs. This resource efficiency can lead to cost savings, as you avoid over-provisioning and allocate resources where they are most needed.

- **Optimized Performance**: Services that scale independently can optimize their performance to handle their specific workload effectively. This can result in faster response times and better overall system performance.

- **Flexibility**: Different services may have varying scaling requirements due to their distinct workloads or usage patterns. Scaling independently provides the flexibility to adapt to changing demands and traffic patterns.

### Negative
- **Complexity**: Managing and coordinating the scaling of multiple services can introduce complexity into your system. Monitoring, configuring, and maintaining the scaling processes for each service may require additional tooling and expertise.

- **Monitoring and Metrics**: We need to establish consistent monitoring and metrics across services to understand their performance and resource utilization accurately. This can be challenging when services have varying scaling requirements.

- **Service Dependencies**: Services with interdependencies may need to coordinate their scaling to ensure that one service doesn't scale excessively and overwhelm its dependencies.

