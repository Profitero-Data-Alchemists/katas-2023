# Concurrency Viewpoint

The Concurrency view serves as a means to elucidate the concurrent elements, state-related structure, and associated constraints within the system. In this context, we aim to illustrate how our solution facilitates **scalability** (capable of accommodating up to 15+ million users and beyond), **elasticity** (able to expand and adapt to unanticipated peak loads during weekends and holiday seasons), and **performance** (ensuring responses are consistently achieved within the 800ms threshold).

To meet these three requirements effectively, it makes sense to leverage the following advantages:

- utilizing a partitioned NoSQL database;
- using topics (including compacted) within a distributed event streaming platform, such as Kafka;
- employing autoscaling instances;
- implementing a load balancer.

## Concurrency Diagram - Level 1

![Concurrency Diagram - Level 1](images/Concurrency-Diagram-Level-1.jpeg)


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

**Note**: In this illustrations, data streams for reading and writing are depicted using black and red arrows, respectively.

### Concurrency Diagram - Level 2 - API Actors

![Concurrency Diagram - Level 2 - API Actors](images/ConcurrencyDiagram-Level-2-API-Actors.jpeg)


As API Actors are required to promptly respond to calls and dynamically accommodate fluctuations in workload, the proposed architecture incorporates scalability through autonomous instances. Load distribution among these instances is achieved through a Load Balancer, while communication with other system components is facilitated via a message queue structured as topics.

### Concurrency Diagram - Level 2 - Data Managers

![Concurrency Diagram - Level 2 - Data Managers](images/Concurrency-Diagram-Level-2-Data-Managers.jpeg)

To efficiently handle varying workloads, Data Updaters are scaled using instances, each responsible for processing tasks from one or more queue topics. These instances are uniquely linked to specific topic partition(s), ensuring seamless data processing without collisions. Data delivery is likewise managed through the instance system and load balancer to ensure optimal distribution.

### Concurrency Diagram - Level 2 - Messaging Actors

![Concurrency Diagram - Level 2 - Messaging Actors](images/Concurrency-Diagram-Level-2-Messaging-Actors.jpg)

The Messaging Actors scaling group employs a compacted topics mechanism, ensuring the uniqueness of records. This uniqueness is vital to prevent redundant updates through the API when reservation statuses change, such as cancellations.

