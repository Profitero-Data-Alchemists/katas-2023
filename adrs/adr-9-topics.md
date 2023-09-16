# ADR 9: Use topics and compacted topics for message handling

**Date**: 2023-09-15

## Status
Proposed

## Context
Choosing the right messaging system is crucial in software architecture and system design for various reasons. A messaging system facilitates communication between different parts of a distributed system, decouples components, and ensures reliable data transmission. 

## Decision
We decided to utilize topics in our messaging system to enable efficient broadcast-style communication, allowing multiple subscribers to receive and react to relevant messages simultaneously. This choice aligns with our application's need to disseminate events, updates, or notifications to various components or services in a scalable and organized manner.

We made the decision to implement compacted topics in our messaging system to optimize message storage and delivery. Compacted topics allow us to retain only the latest message for each unique key, ensuring that our subscribers receive the most relevant and up-to-date information
## Consequences
 
### Positive
- **Scalable Communication**: Topics enable scalable communication, allowing multiple subscribers to receive messages on the same topic simultaneously. This makes it easy to broadcast information to a large number of subscribers efficiently.

- **Flexibility**: Topics provide flexibility in message distribution. Subscribers can choose which topics they want to subscribe to, allowing them to receive only the messages that are relevant to their interests.

- **Reduced Network Load**: Subscribers receive only messages from the topics to which they are subscribed, reducing network traffic and ensuring efficient use of resources.

### Negative
- **Subscriber Management**: Over time, managing subscribers and their topic subscriptions can become challenging, especially as subscribers come and go. Effective subscriber management is essential to maintain system efficiency.

- **Performance Considerations**: The performance of a topic-based messaging system can be impacted by the number of topics, message volume, and the complexity of routing messages to subscribers.

- **Scalability Considerations**: While topics support scalability, they may introduce challenges in terms of load balancing and resource allocation, particularly when dealing with a high number of subscribers or topics.

### Risks
- TBD