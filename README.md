# The Profitero Data Alchemists Architectural Kata by O'Reilly, September 2023

## Team members

- [Ilya Hardzeenka](https://www.linkedin.com/in/ilya-hardzeenka/)
- [Arkadzi Salnikau](https://www.linkedin.com/in/arkadzi/)
- [Denis Klimenko](https://www.linkedin.com/in/den1sklimenko/)
- [Dmitry Trofimov](https://www.linkedin.com/in/tdslinkedin/)
- [Oleg Zubchenko](https://www.linkedin.com/in/rgbd-me/)

## Contents

TODO

## Welcome

Welcome to the Profitero Data Alchemists Architectural Kata run by O'Reilly in September 2023.
We are the architecture team within Profitero, a midsize product company, which gather, enhance, and deliver ecommerce data to our global customer base. 

Our mission is to design and shape the foundation upon which data flows, insights are born, and solutions are built. 

As architects, we understand the power and potential of data to transform industries, drive efficiencies, and unlock new possibilities. We are at the forefront of organizing, securing, and optimizing data ecosystems to unleash their full potential.

In our work we mainly utilize following techniques:
- [Viewpoints and Perspectives Framework by Rozanski,Woods](https://www.viewpoints-and-perspectives.info/)
- [C4 Modeling approach](https://c4model.com/)

## Business Case

### Background
The travel industry is evolving rapidly, with millions of travelers seeking efficient ways to manage their trips. The Road Warrior aims to address this need by providing a user-friendly, feature-rich platform that consolidates travel-related information and enhances the overall travel experience.

### Market Opportunity

- 15 million potential users.
- Competitive advantage through real-time travel updates.
- Analytics data can be monetized for market insights.

### Objective
To develop an innovative online trip management dashboard that offers travelers a comprehensive platform to organize and manage their reservations seamlessly.


### Benefits

- Simplifies trip management for travelers.
- Enhances user experience through real-time updates.
- Facilitates data-driven decision-making for travelers and travel industry stakeholders.

## Requirements

For the original requirenments please follow [Original Requirements](requirements/original_requirements.md)

### Stakeholders

#### Customers or Users
These individuals or organizations use the application to fulfill specific needs or requirements. Their feedback and satisfaction are crucial for the success of the application.

#### Development Team
This includes Product Owners, software/infrastructure engineers, and testers responsible for designing, building, and testing, as well as deploying and maintaining the application in production.
#### Executive Leadership
Senior executives and leadership teams provide strategic direction, allocate resources, and make critical decisions related to the project. They manage the financial, legal, and regulatory aspects of the project.
#### Marketing and Sales Teams
They promote the application, define target audiences, and provide input on features that can enhance marketability.
#### Support and Customer Service Teams
These teams assist users with issues, questions, and feedback, helping to maintain customer satisfaction and loyalty.

### Functional Requirements

#### Email Integration
- The system must periodically poll user emails for travel-related information. 
- It should be able to filter and whitelist certain emails based on user preferences.

#### Integration with Travel Systems
- The system must interface with existing travel systems (e.g., SABRE, APOLLO) for real-time updates.
- The system must interface with existing airline, hotel, and car rental agencies for real-time update.
- Updates must be reflected in the app within 5 minutes after update in Travel API, surpassing competition.

#### Manual Reservation Management
- Users should have the capability to manually add, update, or delete reservations within the system.

#### Trip-Based Organization
- The dashboard should allow users to group reservations by trip.
- Upon trip completion, items should be automatically removed from the dashboard.

#### Social Media Integration
- Users should be able to share their trip information on standard social media platforms.
- An option to share with specific individuals should also be available.

#### Rich User Interface
- Ensure a visually appealing and responsive user interface across all deployment platforms (web and mobile).

#### End-of-Year Summary Reports
- Provide users with comprehensive end-of-year summary reports, including various travel metrics.

#### Analytical Data Collection
- Collect and analyze user trip data for purposes like travel trends, vendor preferences, and update frequency.

#### International Compatibility
- The system must be designed to work seamlessly for travelers across international regions.
- The system must suport multiple languagaes in the user interface


### Non-functional Requirements

#### Availability
- Ensure high system availability, with a maximum of 5 minutes of unplanned downtime per month.
- This results into 99.99% SLA

#### Performance
- Web response time should be limited to 800ms.
- Mobile app should achieve a First-contentful paint of under 1.4 seconds.
- Travel updates must be presented in the app within 5 minutes of being generated by the source.

#### Scalability and Elasticity

- System should handle 2 million active users and adjust accordingly when the load changes
- System should handle reservations from 15M users, each may have 5 or even 10 active reservations, which result in 150M reservations to be refreshed every 5 minutes.

#### Evolvability
- This is growing StartUp - which means we should aim to react to market changes and user feedback almost immediately.


### Assumptions
- All agency APIs, including those for airlines, hotels, and car rentals, provide a standardized integration interface. The primary difference among them lies in the specific endpoints used for communication.

- To cover more users with less effort, we decided to integrate with the top three email providers: Gmail, Outlook, and iCloud Mail. These providers have been prioritized for integration efforts.

- Reservation data can be updated from various sources, including email, agency APIs, and travel systems APIs. This flexibility allows for comprehensive reservation management and real-time updates from multiple channels.

# Proposed Architecture

## Architecture Style

### Driving characteristics
While conducting requirements analysis, various architectural characteristics that held significance for the system were identified.

| Top | Characteristic   | Description                                                                                                                                                                                                                           | 
|-----|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  x  | Performance      | Performance optimization is essential to provide a responsive and user-friendly experience across all platforms.                                                                                                                      |
|     | Responsivenes    | Response time from the web should be limited to 800ms, and mobile should have a First-contentful paint of under 1.4 seconds.                                                                                                          |
|  x  | Scalability      | With 15 million user accounts and 2 million active users per week, scalability is a top priority.                                                                                                                                     |
|     | Elasticity       | The system must efficiently handle a large user base and potential spikes in usage, especially during peak travel times.                                                                                                              |
|     | Interoperability | Integration with external systems (SABRE, APOLLO) and the travel agencies demands an architecture that can evolve to accommodate changes in these systems' APIs or protocols.                                                         |
|  x  | Evolvability     | As user feedback and market trends emerge, there may be a need to introduce new features or services. Evolvability allows for the seamless addition of new functionalities without disrupting existing operations.                    |

The architecture team unanimously agreed on the importance of characteristics like Performance and Scalability. However, when faced with the choice between Elasticity and Evolvability, the team opted for the latter. The rationale behind this decision is that, as a startup, the ability to adapt to user feedback and market dynamics is paramount. The team recognized that the startup's success hinges on its agility to implement changes based on user reactions and shifting market conditions.

In alignment with the top three key characteristics, the team selected an **event-driven architecture** as the most suitable approach.

![Selected Architectural Style](architecture_style/images/arch_style.jpg)


#### Implicit Architecture Characteristics

| Characteristic | Description                                                                                                                                                                                                                              |
|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Availability     | Users must be able to access the system at all times, with a maximum of 5 minutes per month of unplanned downtime. <br/> - High availability is crucial to ensure that travelers can access their trip information whenever they need it. |
| Security      | Filtering and whitelisting certain emails are security measures to prevent unwanted or malicious emails.<br/> - Ensuring the security of user data and interactions is essential, especially in the context of travel-related information. |
| Maintainability | Maintainability is critical for managing and analyzing data over time, adapting to changing requirements. Especially for just launched product on the market                                                                             |
| 

### Functional Viewpoint

> *Describes the system’s functional elements, their responsibilities, interfaces, 
> and primary interactions*

Based on functional requirements we identified main flow our application should support. 
Of coarse there are additional flows but all of them are connected to main one in some way.



<img align="right" width="200" height="200" src="images/legend.jpg">


**Legend for diagrams**

- **Actor**: User or System 
- **Function**: Action performed by Actor
- **Data Element**: May represent an object or it's attribute
- **Constraint**: Functional limitation introduced to simplify system


### Main Flow

The primary flow of our application begins with user registration. Once a user successfully logs in, they have two main options: they can manually enter reservations or set up an email integration to automatically discover reservations in their inbox.
When a reservation is added to the system, it initiates a tracking process using partner APIs to monitor and update its details continuously. Any changes detected are then saved within our system. Users are promptly notified of these changes through push notifications on their mobile devices, email alerts, or pop-up messages on the web.
Since users may have multiple reservations, we provide a feature allowing them to group these reservations into a "Trip." These trips can be viewed by the user or shared via social media as public posts or within the app to another user.


![Main Flow](functional_viewpoint/images/main_flow.jpg "Main Flow")


Additional functional flows described below:

- [Work with User Profile](functional_viewpoint/README.md#work-with-user-profile)
- [Email reservations discovery](functional_viewpoint/README.md#email-reservations-discovery)
- [Work with reservations](functional_viewpoint/README.md#work-with-reservations)
- [Reservations tracking](functional_viewpoint/README.md#reservations-tracking)
- [Work with trips](functional_viewpoint/README.md#work-with-trips)
- [Sharing Trips](functional_viewpoint/README.md#sharing-trips)
- [Work with travel agencies](functional_viewpoint/README.md#work-with-travel-agencies)
- [Analytical reports](functional_viewpoint/README.md#analytical-reports)

### Context Viewpoint

separate doc

### Concurrency Viewpoint

separate doc

### Development Viewpoint

separate doc

### Deployment Viewpoint

separate doc

### Informational Viewpoint

separate doc

### Operational Viewpoint

separate doc





## Costs?


## Architecture decision records
