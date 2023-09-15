# Operational Viewpoint
> *Describes how the system will be operated when it is running on production environment.*

Here we connect the main user flow with the system context. The base things a user do in the application are:
- Login. The app needs to know who going to get help with his trips.
- Setup application. The user has to register a profile in the app to create a context and establish rules.
- Setup a trip. The travel is being planned, thus the user shares with the application what to track.
- Share a trip. It is always useful to be able to share your experience easily.

Then the Road Warrior application steps into play. The application can collect reservations automatically, tracking updates and allows making changes. The system will inform the traveler and let him keep friends up to date.
 To do the staff, the system involves:

The client side:
- The user, firstly.
- The application.

The Road Warrior system:
- All services we put on servers.  

A number of Third party we need to provide our features:
- Identity providers for authentication. So a user can use usual identity for simple login.
- Reservation sources. Travel agencies and travel systems where we can collect actual information and same time and efforts for the user.
- Notifications services, to keep the user informed regardless of the device he is using.
- Social media aggregator. The service connecting us to social networks.

The next schema shows how all it works together to satisfy  our users:


![Base Operational](L1_Operational_RoadWarrior.jpg "Base operations diogram")

