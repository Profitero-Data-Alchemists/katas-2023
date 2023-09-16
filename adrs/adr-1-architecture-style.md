# ADR 1: Architecture style

**Date**: 2023-09-15

## Status
Proposed

## Context
During the requirements analysis phase, the team identified several crucial architectural characteristics for the system: **Performance**, **Scalability**, and **Evolvability**. By using the Architecture Styles Worksheet, the team assessed these key characteristics and ultimately chose the event-driven architecture as the most suitable option.

## Decision
Use an **event-driven architecture** for The Road Warrior system.

## Consequences
 
### Positive
- Event-driven architectures can easily scale horizontally to handle increased loads and traffic, which is crucial for accommodating the project's 15+ million users.
- Event-driven systems excel at delivering real-time updates, such as travel information and notifications, within the app.
- Event-driven architectures are adaptable and allow for the addition of new features and integrations without major disruptions.
- Event-driven systems can easily integrate with external services, including airline and hotel systems, ensuring seamless communication.

### Negative
- Event-driven architectures can introduce complexity, especially in managing event flows and ensuring data consistency.
- Managing a distributed event-driven system requires robust operational practices and monitoring.
- Event-driven development may have a learning curve for the development team, requiring new skills and practices.

### Risks
- Ensuring data consistency across distributed components can be challenging, and improper handling could lead to data discrepancies.
- Complex event routing logic may be difficult to manage and debug, potentially causing errors or delays.