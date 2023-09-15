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