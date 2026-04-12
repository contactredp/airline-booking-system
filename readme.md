# Technical Specifications Document

## 1. Title Page
- **Project Name**: SkyStream Airline Booking System
- **Version**: 1.1 (Revised)
- **Date**: April 11, 2026
- **Author(s)**: Lenin Red Paje, Chinee Marasigan, Shae Padilla, James Ivan Lazo

## 2. Table of Contents
1. [Introduction](#3-introduction)
2. [Overall Description](#4-overall-description)
3. [Visual Mockup Reference](#5-visual-mockup-reference)
4. [Features](#6-features)
5. [Functional Requirements](#7-functional-requirements)
6. [Non-Functional Requirements](#8-non-functional-requirements)
7. [Data Requirements](#9-data-requirements)
8. [External Interface Requirements](#10-external-interface-requirements)
9. [Glossary](#11-glossary)
10. [Appendices](#12-appendices)

## 3. Introduction
- **Purpose**: To develop a mobile-responsive airline booking system focusing on user security and seamless flight management. The application allows users to search for flights, select available itineraries, enter passenger information through a validated booking form, and confirm reservations securely.

- **Scope**: The application will include secure user registration, login, dynamic flight search, a multi-step booking process, and itinerary retrieval. It will not include live payment gateways, real-time flight tracking, or automated check-in systems.

- **Definitions, Acronyms, and Abbreviations**:

  - **Bcrypt:** A library used for secure password hashing and salting to protect user credentials in the database.

  - **CORS (Cross-Origin Resource Sharing):** A security mechanism that allows the backend API to securely communicate with a frontend hosted on a different origin or port.

  - **dotenv**: A zero-dependency module and configuration file used to manage environment variables, ensuring sensitive data like MongoDB strings remain secure and out of the source code.

  - **JWT (JSON Web Token):** An open standard used to securely transmit information between parties as a JSON object, specifically used for user authentication and session handling.

  - **Mongoose**: An Object Data Modeling (ODM) library for MongoDB and Node.js that provides a schema-based solution to manage application data logic.

  - **Node.js**: A JavaScript runtime environment that executes JavaScript code outside the browser.

  - **REST (Representational State Transfer)**: An architectural style for designing scalable, stateless web services and used to create the airline booking system API endpoints.
  
  - **Bootstrap**: An open-source CSS framework used for developing responsive and mobile-first web interfaces.

  - **ERD (Entity-Relationship Diagram):** A visual representation of the database structure, used to map out collections and Object ID relationships within the NoSQL environment.

- **References**: Figma Design System, MERN Stack Development Standards.

## 4. Overall Description
- **Product Perspective**: The application is a standalone single-page web app that simulates the core end-to-end workflow of an airline booking platform.

- **Product Functions**: 
  - User authentication (Secure Registration/Login)
  - Dynamic flight search and filtering
  - Passenger data collection via validated forms
  - Booking storage and retrieval.

- **User Classes and Characteristics**: 
  - **Guest Users/Travelers**: Regular users who search, book flights, and manage their itineraries. Requires an intuitive, mobile-responsive interface for flight management.
  - **Admin Users**: Administrators who manage, flight inventories, user data, and system wide user bookings.

- **Operating Environment**: 
  - **Frontend**: Modern web browsers (Chrome, Edge, Safari) with mobile-first responsiveness
  - **Backend**: Node.js and Express.js runtime environment.
  - **Database:** MongoDB Atlas (Cloud-based).

- **Assumptions and Dependencies**: 
  - Assumes active internet connectivity for database access 
  - MongoDB Atlas cluster is operational.

## 5. Visual Mockup Reference
- **Link or Screenshot**: a visual mockup of the home page, the 'Search Flights' page, and search results made using Figma can be accessed via this link: https://www.figma.com/design/LncFnBdLfb879xl8qkXqM9
- **Design Specs:** 
  - **Primary Palette:** Sky Blue (#A1D0EE), Mint (#99FFFE), and Deep Navy.
  - **Layout:** 12-column grid system for Bootstrap Mobile Responsiveness with a standardized 20px border radius. 

## 6. Features
- **Secure User Authentication:** Provides a robust registration and login system utilizing Bcrypt for one-way password hashing and JWT (JSON Web Tokens) for secure, stateless session management.

- **Dynamic Flight Search Engine:** Features a real-time search interface that allows users to filter available flights by origin, destination, and date, retrieving live data from MongoDB via optimized Mongoose queries.

- **Validated Passenger Data Entry:** Implements a multi-step booking funnel where users provide traveler details through a form-controlled interface, ensuring data integrity before persistence.

- **Mobile-First Responsive UI:** Utilizes a 12-column Bootstrap grid system to ensure the interface is fully responsive and optimized for mobile viewports (320px+) as specified in the Figma design mockups.

- **Persistent Itinerary Management:** Enables registered users to retrieve and view their booking history, leveraging Object ID Referencing within the NoSQL database to maintain a single source of truth for flight and user data.

- **Security & Environment Configuration:** Protects sensitive system information, such as database credentials and API keys, through the implementation of .env files and CORS middleware.

## 7. Functional Requirements
### Use Cases
- **Use Case 1**: Secure User Registration
  - **Title**: User Registration with Password Encryption
  - **Description**: A new user creates an account to access booking features.
  - **Actors**: Guest User
  - **Preconditions**: User is on the Registration page.
  - **Postconditions**: User account is created; password is encrypted via Bcrypt in MongoDB.
  - **Main Flow**: User enters registration details > Clicks 'Register' > System validates input and hashes password > Account is saved.
  - **Alternate Flows**: User provides an existing email or leaves fields blank > System displays validation error.

- **Use Case 2**: End-to-End Flight Booking
  - **Title**: Flight Search, Selection, and Reservation
  - **Description**: A logged-in user searches for available flights and completes a reservation.
  - **Actors**: Registered User
  - **Preconditions**: User is authenticated via JWT and on the Search page.
  - **Postconditions**: A booking record is created in MongoDB referencing the User and Flight IDs.
  - **Main Flow**: User enters search criteria > System displays results from MongoDB > User selects a flight > User enters Passenger Details > User confirms booking.
  - **Alternate Flows**: No flights match criteria > System displays "No Flights Found" message.

- **Use Case 3**: Itinerary Retrieval
  - **Title**: View and Manage Bookings
  - **Description**: User views their previous and upcoming flight reservations.
  - **Actors**: Registered User
  - **Preconditions**: User has at least one existing booking record.
  - **Postconditions**: System displays stored booking details.
  - **Main Flow**: User navigates to 'Manage Booking' > System retrieves data using Mongoose populate() > Details are displayed on a mobile-responsive dashboard.

### System Features
- **Feature 1**: Secure User Registration & Authentication
  - **Description**: Allows users to create accounts and securely log in to access booking management tools.
  - **Priority**: High
  - **Inputs**: User's name, email, password, and contact number.
  - **Processing**: The server receives data, hashes the password using Bcrypt, and stores the new user document in MongoDB.
  - **Outputs**: Successful account creation and session establishment via JWT.
  - **Error Handling**: Displays validation errors for missing fields, invalid email formats, or existing accounts.

- **Feature 2**: Dynamic Flight Search
  - **Description**: Enables users to query available flights based on specific travel criteria.
  - **Priority**: High
  - **Inputs**: Origin, destination, departure/arrival dates, class, and passenger count.
  - **Processing**: The system validates inputs and performs a Mongoose query to find matching documents in the Flights collection.
  - **Outputs**: A filtered list of available flight cards with schedule and price details
  - **Error Handling**: Displays an "Empty State" or "No Results Found" message if no flights match the criteria.

- **Feature 3**: Validated Booking Form
  - **Description**: Collects and validates detailed passenger and contact information to finalize a reservation.
  - **Priority**: High
  - **Inputs**: Passenger names, contact details, and selected flight reference.
  - **Processing**: Validates data integrity and creates a new document in the Bookings collection using Object ID Referencing.
  - **Outputs**: A booking confirmation summary and unique reference number.
  - **Error Handling**: Show specific errors for invalid data formats or missing mandatory passenger fields

  - **Feature 4**: Manage Booking (Itinerary Retrieval)
  - **Description**: Allows registered users to retrieve and view their existing flight reservations.
  - **Priority**: High
  - **Inputs**: User authentication token or specific booking reference number.
  - **Processing**: The system finds and returns stored booking information using Mongoose populate() to link user and flight data.
  - **Outputs**: Detailed itinerary view including departure/arrival times and status.
  - **Error Handling**: Show a "Booking Not Found" error if the reference is invalid or the session has expired.


## 8. Non-Functional Requirements
- **Performance**: 
  - The system should run smoothly for the end user with searches and account registration only taking a few seconds of wait time.
- **Security**: 
  - Sensitive information sent to the MongoDB database must be encrypted for security.
  - Using the dotenv Node.js module, sensitive data used by the server must be stored in a .env file instead of being stored in plain text.
- **Usability**: 
  - The UI should not be confusing to the user in order to maintain a smooth experience and clear navigation.
- **Reliability**: 
  - Both the server and the database must be running in order for the system to function and the end user's requests answered.
- **Supportability**: 
  - The code just be readable with clear comments for maintainability.
  - Both the code in the front end written in HTML, CSS, and Bootstrap as well as the back end code in JavaScript consisting of the server, API, and operations with the MongoDB database must be clearly segregated and labeled.


## 9. Data Requirements
- **Data Models**: This is the format used by the documents stored in the collections in the MongoDB database. Field names and data types are given.
  - **Users**: { _id (objectID), firstName (String), lastName (String), email (String), password (Hashed String), isAdmin (Boolean), mobileNumber (String), bookedFlights (Array of ObjectIDs) }
  - **Airlines**: { _id (objectID), name (String), countryOfOperation (String), flights (Array of ObjectIDs) }.
  - **Flights**: { _id (objectID), airlineID (ObjectID ref: Airlines), origin (String), destination (String), departureTime (Date), arrivalTime (Date), class (String), price (Number), isActive (Boolean), seatsAvailable (Number) }.
  - **Bookings**: { _id (objectID), userID (ObjectID ref: Users), flightID (ObjectID ref: Flights), passengerDetails (Object), numberOfSeats (Number), totalAmountPaid (Number), paymentMethod (String), bookedOn (Date) }.
                                                                                                                                                                                                                                                
- **Database Requirements**:
  - **Database Architecture:** The system utilizes MongoDB, a document-oriented NoSQL database, to provide high scalability and flexible data structures for flight and user management.
  - **Password Security: The `password` field must be noted as a **Hashed String** to reflect the use of Bcrypt for security.
  - **Object ID Referencing:** The `bookedFlights` in the **Users** collection and `flights` in the **Airlines** collection should explicitly store **ObjectIDs** rather than simple Strings to support Mongoose `populate()`.
  - **Passenger Details:** A `passengerDetails` object is added to the **Bookings** model to capture traveler information (Name/Contact) as specified in your **Booking Form** feature.
  -  **Flight & Airline Relationship:** Each document in the Flights collection includes an airlineID linking back to the Airlines collection to associate flights with carriers
  -  **Data Aggregation:** The server, built with Node.js and Express, utilizes the Mongoose `populate()` method to retrieve and merge related data across collections during search and itinerary retrieval.

- **Data Storage and Retrieval**: Handled via Node.js/Express utilizing the Mongoose ODM for MongoDB communication.
- **Entity-Relationship Summary:**
  - A One-to-Many relationship exists between Airlines and Flights.
  - A Many-to-Many relationship is managed between Users and Flights via the **Bookings** collection, which acts as the reference link.

- **ERD Implementation:** The physical database structure is implemented according to the Entity-Relationship Diagram (ERD), which defines the primary connections between user accounts, flight schedules, and booking records. 
**ERD Link:** The Entity-Relationship Diagram for the system can be viewed via this link: https://drive.google.com/file/d/1E4F9EyBhdrqAfFI_aChv3gZBbX0LSGDR/view?usp=sharing

## 10. External Interface Requirements
- **User Interfaces**: These are the web pages the end user can access. The design follows a mobile-first approach with a 20px border radius and a Navy/Sky Blue palette. (Refer to the Figma mockup for visual details).
  - **Homepage:** Features the main search bar and hero section.
  - **Registration Page:** Includes validation for secure account creation.
  - **Login Page:** Secure gateway for registered users.
  - **Search Page:** Displays dynamic results retrieved from MongoDB.
  - **Booking Management Page:** Personal dashboard for viewing itineraries.
- **API Interfaces**: The backend is powered by Node.js using the Express.js framework to build a RESTful API. This serves as the communication bridge between the React frontend and the MongoDB database.
- **Hardware Interfaces**:
  - The system is accessible via any standard personal computer or mobile device with internet connectivity.
  - The application is designed to be hosted on a cloud-based server environment.
- **Software Interfaces**: 
  - **Node.js:** The runtime environment for the server.
  - **Mongoose ODM:** Facilitates communication between the server and the MongoDB database.
  - **Bootstrap:** CSS framework used to maintain responsive design across all software viewports.

## 11. Glossary
- **Application**: A software program designed to perform specific tasks for users or other systems.
- **Browser**: A software application used to access and display content on the World Wide Web (e.g., Chrome, Firefox, Safari).
- **Client**: A system or application that requests services or resources from a server.
- **Server**: A computer or program that provides services, data, or resources to clients over a network.
- **Document**: A record in MongoDB composed of key-value pairs and stored in BSON format.
- **Collection**: A group of MongoDB documents, analogous to a table in relational databases.
- **Field**: A key-value pair within a MongoDB document representing a specific data attribute.
- **Data Type**: A classification that specifies the kind of value a variable or field can hold, such as string, number, boolean, or date.

## 12. Appendices
- **Supporting Information**:
  - **Trello Board:** Used for task management, feature definition, and tracking development progress among team members.
  - Trello Board Access Link: https://trello.com/b/rdefYDeD/mcp-side-project-1

- **Revision History**:
  - v1.0 (2026-03): Initial project setup and core feature definition (Lenin Red Paje).

  - v1.1 (2026-04): Technical refinement, integration of Bcrypt/JWT security, design specification updates, and data model normalization (Chinee Marasigan).
