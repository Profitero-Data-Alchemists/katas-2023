# ADR 2: API layer as single point of contact for all user interfaces

**Date**: 2023-09-15

## Status
Proposed

## Context
The Road Warrior is a startup project aiming to develop the next-generation online trip management dashboard. The primary goal of this project is to provide travelers with a unified platform where they can view all their existing reservations, organized by trip, accessible through both a Mobile App and a Website. The project anticipates significant usage spikes, and the team can observe challenges related to system unavailability during peak usage, system freezes, and data lost.

## Decision
In anticipation of potential challenges and with the aim of establishing a stronger and more scalable architecture, the decision is to propose an **API layer as the primary interface** for both the Mobile Apps and Website.

## Consequences
 
### Positive
- By introducing the API layer, the system can better handle usage spikes, ensuring smoother user interactions and reducing the risk of unavailability during peak times.
- A unified API layer simplifies the process of attaching roles and permissions to specific endpoints, bolstering security and access control.
- The API layer creates an abstraction layer toward downstream services, increasing the system's flexibility.


### Negative
- The addition of the API layer introduces a new level of complexity to the architecture, potentially complicating development and maintenance efforts.

### Risks
- Managing the complexity of the API layer may pose challenges during development, potentially leading to delays or unexpected issues.
- The API layer may introduce some performance overhead due to the additional layer of abstraction.