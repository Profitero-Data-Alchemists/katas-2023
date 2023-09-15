# Information Viewpoint

# Purpose

This document describes Data Schema for the solution and provides rationale for
decisions taken.

# Overview

![Data Schema](information_viewpoint/images/information_viewpoint_1.jpg)

Functionally, the application provides several feature groups, including:

- User Management
- Reservation Tracking
- Access Control
- Report Generation
- User Notification

# Glossary

TODO: pulling vs discovering

# Entities

# User

User table contains all the information that is related to specific user.
The data is denormalized, and dynamic fields with multiple records are present
in the table. This decision provides better performance and is viable since
the data in the user table is not participating in any busines-critical
transactions.

## Fields:

- ID - primary key
- E-mail - the e-mail that user registered with and from which the reservations
  will be discovered
- E-mail filtering rules - configuration of search terms and other filters to
  use when crawling for e-mails. The field has a fallback value set up to match
  all possible travel-related emails
- Personal Info - User-related personal data
  - Name - for polite user addressing
  - Location - home address of the user, for emergency issue resolution
  - Language - preferred UI language
  - Contact Info - for emergency cases and issue resolution
- Notification settings - preferences regarding frequency, importance and
  delivery method for the in-app notifications.

Inbound Links
- Preferred Agency ID - travel agency that should be used for quick issue
  resolution

Outbound Links
- Trips (Through "Trip Access Control List" join table)
- Reservations
- Notifications
- User Reports

## Reservations

This entity represents single trip event, (optionally) identifiable and
trackable in other travelling systems.

Table has Surrogate Key for performance and ability to manage reservations that
are NOT available in any other tracking system (e.g. privately agreed transfer
or hotel stay).

### Fields
- WIP

# Changelog

## Revision 0 (Initial)
