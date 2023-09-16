# Context viewpoint
Context viewpoint is intended to describe the relationships, dependencies, and interactions between the system and its environment (people, systems, and external entries with which it interacts).

Using the C4 model system decomposition is explained step by step. The most high-level explanation of the system presented on the schema: ``Level 1 - System Context``

## Level 1 - System Context

![Level 1 - System Context](/context_viewpoint/images/Level-1-System-Context.svg)

In the middle of the schema here - is the software system for a new startup called "Road Warrior". This diagram shows the parties the system iterates with. Here they are:

On the left side:
 * ``User`` that interacts with the system through the web application in the browser or through the mobile application.
 * ``Support user`` that does special actions in application such as maintaining available Travel Agencies or generating a vendor analytical report.

On top:
* ``Identity API`` is used to verify the identity of the user who logs into the application. It also provides basic info about the user, that the user agreed to provide during the registration.
* ``User Email API`` provides the ability to discover new reservations by user email polling (if allowed by the user).

On the right side:
* ``Travel Agencies API`` provides updates for reservations that were added by users or discovered in user email.
* ``Travel Systems API`` also provides updates and details for reservations.

On bottom:
* ``Social Media`` provides APIs to share the post in user's social media accounts.
* ``Notifications`` are used to send user notifications such as emails or push notifications for mobile applications.

## Level 2 - Software System - Road Warrior
The topmost parts of the "Road Warrior" software system are presented on ``Level 2``. This level expands the internals of the software system presented in the previous schema without going too much into detail for now.

![Level 2 - Software System - Road Warrior](/context_viewpoint/images/Level-2-Software-System-Road-Warrior.svg)

Once the system environment stays the same focus of this schema is on Software System internals. Component ``Road Warrior UI`` represents a web application, that hosts the frontend part of the web application and all its static content. Common for the web application and mobile applications backend available as REST API component ``Gateway API``. It hosts all backend requests, including ones for login and register functions. To verify user identity ``Gateway API`` refers to the ``Identity API``.

Reservations can be added manually by the user via one of the Road Warrior applications or discovered automatically by the email poller. Once the reservation is registered in the system, it will be updated by one of the trackers combined into container ``Trackers``. Every tracker polls the updates for the reservation from the ``Travel Systems API`` or ``Travel Agencies API``.

Once there is a change in the reservation, the system initiates notification of the user about the change using supported ways that are described in the container ``Notificaton Publisher``. It prepares the notification according to the user notification settings and sends a notification to the user using external ``Notifications`` APIs.

Every Trip that a user owns in his account can be shared with other users by posting a message on social media or by sharing the Trip with targeted people. Both of these sharing mechanisms are implemented in container ``Sharing``.

All information about User data, Trips, Reservations, Travel Agencies and Notifications is stored and shared with readers by container `Data Readers/Updaters`.

## Level 3 - Containers

Information presented on the third level of context viewpoint drilled down to the internals of the containers from the previous level.

### Level 3 - Container - Emails Tracker

![Level 3 - Container - Emails Tracker](/context_viewpoint/images/Level-3-Container-Emails-Tracker.svg)

Implementation of Email tracker based on the use of the Compacted topics. Emails that are used to track new reservations are registered by microservice `Email Tracking List Editor` into compacted topic `Email Addresses`. Each email from the topic is processed by the `Emails Tracker` microservice, which includes:
1. polling new emails compared to a previous state (if any),
2. store new emails to parse to `Unparsed User Emails` topic,
3. store the last processed email timestamp back to the `Email Addresses` compacted topic.

That way on each new iteration the `Emails Tracker` microservice will be able to identify and process new user emails.

Emails from the `Unparsed User Emails` are captured by the microservice `Emails Parser` which parses the content of the user emails to discover new reservations. Found new reservations stored in the `Email Reservations` topic. The microservice `Reservations Updater` processes reservations from the topic:
1. Stores reservation info in `Reservations Data Reader/Updater` service,
2. Sends an update to the `Notification Publisher` service.

### Level 3 - Container - Reservation Trackers

![Level 3 - Container - Reservation Trackers](/context_viewpoint/images/Level-3-Container-Reservation-Trackers.svg)

Implementation of `Reservation trackers` based on the use of the Compacted topics. The reservations that should be tracked are picked by the `Reservation Tracker` microservice and sent to one of the Compacted topics depending on the reservation type. Each type of the topic being processed by Tracker for a specific reservation type and sent to the external Travel System or Travel Agency API for reservation updates. Any existing update stored back to the corresponding Reservations compacted topic and being processed by the `Reservation Updater` microservice. It updates the reservation information in the system by:
1. Sending the Reservation update to the `Reservation Data Reader/Updater` service,
2. Sending the Reservation update to the `Notification Publisher` service to notify the user about the change.
