# Information Viewpoint

# TODO

- refactor user-reservation relationship. the owner relation is NOT done through
  the access control table, since it's a special case.
- Update image when doc is approved
- update depths of headers to correct values
- cross-link mentioned entities and markdown headers

# Purpose

This document describes Data Schema for the solution and provides rationale for
decisions taken.

# Overview

![Data Schema](./images/information_viewpoint_1.jpg)

Functionally, the application provides several feature groups, including:

- User Management
- Reservation Tracking
- Access Control
- Report Generation
- User Notification

Data Schema mirrors that separation

# Glossary

TODO: pulling vs discovering
TODO: reminder: analyse with "why"



# Entities


## User

User entity represents end user of the app and contains all the information that
is related to specific user.

### Decisions
- The data is stored in one table with dynamic schema fields with multiple
  records are present in the table. This approach provides better performance,
  easier extensibility, and is viable since the data in question is not being
  modified in any busines-critical transactions.

### Fields:

- ID - surrogate primary key for performance and easier GDPR compliance.
- E-mail - user's email. Used for authentication and reservation updates
  discovery and tracking.
- E-mail filtering rules - configuration limiting scope of scraped emails. Used
  for improved privacy and performance.
- Personal Info - User-related personal data
  - Name - for polite user addressing
  - Location - home address of the user, for emergency issue resolution
  - Language - preferred UI language
  - Contact Info - for emergency cases and quick issue resolution
- Notification settings - preferences regarding frequency, importance and
  delivery method for app notifications for convenience.

### Outbound Links
- Preferred Agency ID - travel agency that should be used. Used For quick issue
  resolution

### Inbound Links
- Shared Trips (Through "Trip Access Control List" join table) - trips shared with
  with the user by other users. Used for authorization and access control.
- Owned Trips (directly by Owner ID) - trips created and owned by the user
- Reservations - reservations created and owned by the user. Used for
  authorization.
- Notifications - for tracking read receipts.
- User Reports - reports intended for the user. For recipient tracking


## Reservation

Reservation entity represents single trip event, (optionally) identifiable and
trackable in travel agency reservation management systems. 

### Decisions
- Table has Surrogate Key instead of natural Personal Name Record key for
  performance, easier GDPR compliance and ability to manage reservations that
  are NOT available for tracking in any other tracking system (e.g. privately
  agreed car transfer or hotel stay).
- The potential load of updatign the itinerary of the trip is high, that's why
  the core itinerary data is stored in a separate data collection, see
  `Reservation Itinerary`.

### Fields
- ID - surrogate key
- Tracking Info - natural key. Personal Details that are used to look up
  reservation details in reservation tracking systems.
- GEO Info - information about the geographical locations involved in a
  reservation. Field is dynamic to allow for easier extensibility and support of
  different reservation types. Used for user convenience and analytical
  purposes.
- Financial Info - financial details of the trip. Used for analytical purposes
  for user and vendor reports.
- Name - in-app display name of the trip
- Status - enumeration, used for tracking lifecycle of the reservation. E.g
  active(tracked), cancelled(by vendoer), removed(by the user), finished(by
  passage of time) etc.
- Description - free-form description of the reservation. A space for user to
  specify additional information.
- Data Source - information about where the reservation was discovered. Used for
  Investigation purposes.

### Outbound Links:
- Agency ID - travel agency managing the reservation. Used for issue resolution
  and for data tracking
- Trip ID - trip that the reservation belongs to, Is used for grouping, see
  `Trip`
- UserID - The creator and owner of the reservation. used for Authorization.

### Inbound Links
- Reservation Itinerary - Historical versions of the trip itinerary details.
  Extracted to separate collection for performance.


## Trip

Trip entity represents a group of reservations for convenience. Also trips
can be shared with other users, to scope sharing feature data access, 

### Decisions
- Each user has a service "Unassinged" trip ontaining all not-yet-assigned
  reservations. This trip is used to unify the flow of working with reservations
- User owning the trip is still tracked via directo reference, for simpler
  investigation.

### Fields
- ID
- Description - A space for user to specify additional information.
- Dates - start and end date of the trip. Is used for convenience purposes and
  easier automatic grouping of reservations.
- Is Unassinged - Special flag to mark the unassigned reservation. Exactly one
  unassigned trip per user should exist.

### Outbound Links
- Owner - User creator and owner of the trip. Used for authorization.

### Inbound Links
- Users (Through "Trip Access Control List" join table) - list of users and
  corresponding roles they have been granted for the trip.


## Trip Access Control
Trip Access Control List entity holds information on access of users to the
trips that have been shared with the by other users. Used solely for
authorization.

### Fields
- Role - enumeration with different access levels for the trip. Levels of access
  range from "full read access" used for co-travellers to "trip dates and
  locations" for case of sharing data with wider and/or less trusted audiences,

### Outbound Links
- User - user for whom the access role applies
- Trip - trip to which the access control rule applies


## Travel Agency

Travel Agency entity contains reservation-tracking and issue resolution
information.

Decisions:
- Dat acollection is maintained up to date by support engineers.

### Fields
- ID
- Name
- Description - info about the travel agency. Is used for internal purposes and
  to be presented to user. Contains contact information.
- API URL - URL to be used for tracking resevation updates for this agency

### Inbound Links
- User - user for whom this travel agency is preferred for the issue resolution


## Notification

Notification entity records deliverance of app notifications to the users.

### Fields
- ID
- Status - delivered or not
- Type - to allow for identification of different interpretation in code and richer
  content for the user.
- Type Specific Data - dynamic field, data to present to the user.

### Outbound Links
- User - the intended recepient of the notification


## User Report

Entity representing Reports - datasets collected for presenting to the user,
e.g. the annual report.

### Decisions

- In case of high peak loads, reports can be moved to a separate analytical
  database, for better  performance.

### Fields
- ID
- Type - for identifying different types of reports in the code and richer
  content for the user
- Type Specific Data - additional info native to the report type. Is interpreted
  by the system.
- Data URI - link to the actual data of the report. Used to allow for storing the
  data in a more performant storage, e.g. BLOB storage.

### Outbound Links
- User ID - the intended recipient of the report.


## Vendor Report

Entity represents dataset record of aggrecated analytical data generated for the
vendors, e.g. travel agencies. All the data in the dataset is used for
analytical purposes.

Decisions:
- In case of high peak loads, reports can be moved to analytical database, for
  better performance
- Only data storage is described. Data export feature is not addressed, and
  should be refined with the product owner
- The primary key is chosen to mask personal info, but yet provide useful
  granularity for the consumer
- Stats are given for example, and should be rigorously refined with product
  owners

### Fields

Primary Key - scope of the statistical data in the record
- Time period - date range for the scope of report stats
- Agency - agency filter for reservations
- User Geo Region - country or state-wide range of user's home locations
- Reservation Geo Region - country or state-wide range of Reservations locations

Stats
- Financial Stats
  - Average per-user bill
  - Total money spent
- Updates Stats
  - Re-scheduling Rate
  - Replacement Rate
  - Cancellation Rate

# Changelog

## Revision 3

Rewrite All entity sections with "why" question in mind.

## Revision 2

Rewrite User, Resrvation sections with "why" question in mind.

## Revision 1

Draft information for all entities added.

## Revision 0
- Initial

