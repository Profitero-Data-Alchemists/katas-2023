# ADR 13: Use Multi Zone Infrastructure

**Date**: 2023-09-15

## Status
Proposed 

## Context
Choosing the right infrastructure is critically important for the application due to high Availability requirements, and it can have a significant impact on the success and efficiency of the application.

## Decision
To attain the desired SLA, we propose initially employing multi-zone deployment within a single region. We recommend against multi-region deployment due to added network costs for inter-region communication. Access to the network should be facilitated through Public Load Balancers with built-in firewalls capable of handling external threats and internal failures.
## Consequences
 
### Positive
- **High Availability**: One of the primary benefits of a multi-zone infrastructure is improved high availability. By distributing your resources across multiple zones, you reduce the risk of downtime due to hardware failures, network issues, or other infrastructure-related problems. If one zone experiences a failure, traffic can be automatically routed to a healthy zone.

- **Disaster Recovery**: In the event of a major disaster or outage in one zone, a multi-zone setup can facilitate disaster recovery by allowing you to quickly switch traffic and operations to a different zone with minimal downtime.

- **Improved Performance**: Distributing resources across multiple zones can improve overall system performance. You can serve users and customers from the zone geographically closest to them, reducing latency and improving response times.

- **Scaling and Load Balancing**: Multi-zone architectures often support load balancing across zones, which allows you to distribute incoming traffic evenly and efficiently among multiple servers or instances. This makes it easier to scale your infrastructure to handle increased workloads.

### Negative
- **Complexity**: Managing a multi-zone infrastructure can be more complex than a single-zone setup. It may require specialized tools and expertise to ensure proper configuration, monitoring, and maintenance.

- **Cost**: A multi-zone architecture can be more expensive due to the additional hardware, networking, and data transfer costs associated with running resources in multiple zones.

- **Latency**: While multi-zone architectures can improve performance for users in different geographic locations, they can also introduce latency for applications that require frequent data synchronization between zones.
