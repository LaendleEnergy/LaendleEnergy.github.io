# LaendleEnergy Documentation

## Table of contents

1. [Coaching documentations](#coachings) \
   1.1. [Project coaching (Wolfgang Auer)](#auer) \
   1.2. [Data Management (Peter Reiter)](#peter) \
   1.3. [Advanced data analytics (Sebastian Hegenbart)](#sebastian) \
   1.4. [UI (Walter Ritter)](#walter) \
   1.5. [Web (Daniel Rotter)](#daniel) \
   1.6. [Security (Armin Simma)](#armin)
2. [Technical documentation](#technical) \
   2.1 [CI/CD & DevOps](#devops) \
   2.2 [Authentication](#auth) \
   2.3 [Embedded Device](#embedded) \
   2.4 [Testing](#testing) \
   2.5 [DataCollector: Database First](#datacollector) \
   2.6 [Exploratory Data Anaylsis](#eda) \
   2.7 [Communication between microservices](#communication)
3. [Project progress report](#projectprogress) \
   3.1. [Sprint 0](#sprint0) \
   3.2. [Sprint 1](#sprint1) \
   3.3. [Sprint 2](#sprint2) \
   3.4. [Sprint 3](#sprint3) \
   3.5. [Sprint 4](#sprint4) \
   3.6. [Sprint 5](#sprint5) \
   3.7. [Sprint 6](#sprint6) \
   3.8. [Sprint 7](#sprint7)
4. [Future work](#futurework)

## 1. Coaching documentations <a name="coachings"></a>

### 1.1. Project coaching (Wolfgang Auer) <a name="auer"></a>

---

**Project idea** (21.09.23)

_Author_: Bianca

Further procedure:

- Define application scenarios
- Define requirements
- Define target group (Who are we doing this for?)
- Create 1-2 proto-personas
- Create paper prototype

---

**User stories** (12.10.23)

_Author_: Bianca

_Proposal_: The original idea was to set sliders on an energy consumption curve to indicate which device was used in a particular curve. Recommended was to do this the other way around, so to first start a device and then record its usage so that the system immediately learns which device has which consumption. This procedure is recommended because it might make more sense in terms of usability, but it does not have to be implemented this way.

_Recommended further use cases_:

- Notes on the energy efficiency of the appliances in question: e.g., "Your hair dryer uses x kWh, which is y kWh more than the average hair dryer needs" or "If you replace this appliance with this appliance, you could save yourself x kWh"
- Problem detection based on the collected data for a device would add some practical value to the application. One example for this would be to monitor energy consumption of a washing machine. If it uses more electricity then usual, it might be calcified.
- Suggestions on when to turn on which devices, like e.g. "PV system brings x electricity right now so it would be reasonable to turn on applicance y now", or turn on washing machine in 3h.

---

**Submission: Presentation and Documentation** (11.01.24)

_Author_: Bianca

- Document what else could be done in the future (which we didn't have time to do)
- When presenting: First introduce what it is about before moving on to the technical part (What did we want to do? What did we do?)
- Concentrate on the essentials
- Technical quality for the existing should be right
- Define what is left out and document this as a possible extension
- Presentation requests:
  - Overview (What we wanted to do, what has been done, what is missing)
  - Address each topic area from each coach

### 1.2. Data management (Peter Reiter) <a name="peter"></a>

---

**Data model** (16.11.2023)

_Author_: Dominik

- Separate DeviceId and household
- SmartMeter as a separate entity (MeterIndividual, instance of a meter)
  - Theoretically, a household can theoretically have several SmartMeters
  - SmartMeter type as an attribute (e.g. Kaifa)
- Measurement is a MeterReading
  - Can have several property values
  - Has a unit, date, numerical value
  - "Original can be left in a first step"
- Encode address according to ISO 19160-4-2017 Address Standard
- Add user contact data (email, number, ...)
- Admin not as a subtype of User but as a role of User
- Contextual role
- Revise measurement-label connection
- Labeling is "sufficient" for our team
- Look at multi-tenant handling
- "Does each household get its own database or a separate schema in a database or does all data end up in one database?"
- Look at partitioning at timescale
- Strategies:
  - One table and partitioning
  - Multiple tables within one schema
- Disadvantage: Cannot be distributed to different computers because they all have one schema
  - A separate schema with separate tables for each user
- Disadvantage: Still everything on one database
  - Each user gets their own database
- Link via foreign data mapper
- Master databases
- Labels without a time period "don't hurt"
- Describe in the documentation what is meant by the individual entity types
- Check what kind of model this is

### 1.3. Advanced data analytics (Sebastian Hegenbart) <a name="sebastian"></a>

---

**Labeling** (16.11.2023)

_Author_: Bianca

- Concept for simultaneous labeling? Possibly work out over time when what is running
- Noisy data will be a problem  questionable how much ML is possible in a meaningful way; do not rely on it
- 2x already started washing machine  Predict from experience how long the machine will run
- Store 1-n labels for each measured value  probably too inefficient; consider whether only the start and end should be recorded
- Incorporate the option to edit the data (labels) retrospectively

### 1.4. UI (Walter Ritter) <a name="walter"></a>

---

**Paper Prototype** (31.10.2023)

_Author_: Dominik

- Button placement guide (?)
  - Define primary / secondary buttons
  - "Third" unimportant subordinate buttons only as text links
- Home page:
  - Information / benefits, what the application does, why you should use the application
  - Designed as a landing page:
    - Problem description
    - Problem solution
    - 1 sentence is sufficient
- Registration page:
  - Password criteria? None defined so far
  - (Username must be unique) -> Name instead of username. When is the check carried out?
  - If the user name is used, suggest another user name when clicking on it?
- Electricity tariff selection page:
  - Step display, where you are in the registration process
  - Electricity provider dropdown (initially only with vkw)
  - Introduce a meter number check to see if it already exists
- Home page:
  - Introduce comparison diagram (how much have I saved / consumed more in comparison)
  - Pay attention to axis descriptions
  - Use terminology of target persons
  - Arrangement of diagrams: Display important diagrams first, e.g. if saving electricity is important, display electricity costs and savings first
  - Create diagram Process: Define target. Which metrics are available to me for the target. Define exactly what the diagram should show -> Implementation via Goal Query Metrics model.
- Labeling page:
  Label name: "Assign consumption"
  Color the curve / area when selecting the time period so that consumption is easier to see
  - Revise the time axis- Competition page
  - Edit button should only be visible to the admin
  - In the ranking list, write how often you have labeled
  - Create a button for labeling from the ranking list
  - Highlight benefit for labeling
  - Overview dialog for the missing first place such as "Hey Walter, assign 5 more labels to get the pizza"
  - Rename navigation button: instead of "Comparison" something else, e.g. "Household members ranking", "Reward"?
- Personal information page
  - Save highly sensitive data securelyo Mark optional fields
  - Mark page explicitly as optional implementation
  - Add an additional password field for repetition
  - Delete account stands out too much as a subordinate button Perhaps only as a text link with color coding (no red!)o Group data between account, contact and household data On the same or a separate page?- Current members pageo Highlight optional fieldso Rename to household members instead of just memberso Highlight household members and profile difference
- Create your own profile
  - More information
    - Which household have I been invited to Previously known information
    - Highlight benefit againo "Join household" instead of "Create profile" as a button- Devices pageo Overview of appliances directly on the appliance page
- Energy efficiency diagram
  - Sell this as a benefit when registering
  - Display devices with "catch-up requirements" first
  - Perhaps display several appliances at the same time (e.g. 5 worst performing)
  - Highlight the difference between appliance category(/type) and appliance label
- Electricity saving page
  - Optional: Suggestions for saving electricity
  - Possibly place tariff recommendation under consumption, as it is not really part of the "Save electricity" tab
  - Define whether the focus is on saving electricity or money
  - Report button placement debatable (because it does not only belong to electricity saving)
  - Describe what a report is
- Electricity consumption page:o Alternating presentation of energy and costs
  - Insert tariff recommendation there
  - Cost diagrams with tariff dropdown

---

### 1.5. Web (Daniel Rotter) <a name="daniel"></a>

---

**Microservices** (02.11.2023)

_Author_: Bianca

_Preparation:_

- Own database for Data Preparation Service?
- ML service -> read only to time series database or own DB?
- Does it make sense to split the microservices in the way we did?
- When to use REST/events?

_Documentation:_

- Consideration: Link measurement to the user?
  - Child consumes so and so much power while playing with toy x
  - Answer questions such as: how much more electricity does it need if you have 1-3 children?
- Data Collector & Data Preparation very closely coupled (Data Preparation always accesses Data Collector), possibly ML service later too
  - Data Preparation Service should not always fetch all data from DB -> becomes a lot if you get new data e.g. every 5 seconds
  - Suggestion: merge (if synchronous) or use CQRS (asynchronous, create new view)
    - Advantage of CQRS: if Data Collector is down, something can still be displayed
  - Data Preparation Service should aggregate data immediately
    - Note: See what Timescale can do, not in the frontend (use hypertables)
  - E.g. also precalculate data for the predefined time periods (annual, monthly, weekly, daily)
  - Build exactly the views for the specified use cases
- Ideally: each microservice = 1 team
- Consider performance and availability (possibly perform load test, look at caching layer at timescale)
- Data Preparation to Data Collector: Work with events (if you separate them at all; see what Timescale can do)
- Events are preferable, but are more complex
- REST only if it is not so bad if a service on which you depend fails (pay attention to the specific requirements)
- CAP theorem for the DB (only two can be fulfilled)- Play through user stories (to which microservice does the request go, which microservices are involved, ...)
- Play through user stories (to which microservice does the request go, which microservices are involved, ...)
- 4+1 scheme (1 = scenarios): Based on this scheme, think about what is necessary

---

### 1.6. Security (Armin Simma) <a name="armin"></a>

_Author_: Lucas, Dominik

_Preparation:_

- Embedded:
  - Password generation
  - transmit password from embedded device to server
  - authentication
- JWT Authentication:
  - What should be the expiring date of the JWT token?
  - Should the token renew itself automatically?
  - How should passwords be transmitted from client to server?
    _Documentation:_
- Embedded:
  - Patrick: password generation on embedded device -> password and id get readout in production teststage and saved to server
  - Patrick: Use own implementation instead of mqtts (to much overhead)
    - own challenge-response implementation ontop of udp
  - Authentication state befor coaching:
    - sessionkey derived using aes-gcm to encrypt the challenge with the client key
  - Armin: use hmac for sessionkey derivation
- JWT Authentication:
  - One should have two tokens
    - Access token:
      - A short-lived token that is used to make authenticated requests to microservices
      - It should have a short lifespan
    - Refresh token:
      - A long-lived token that is used to obtain new access tokens when the current access token expires
      - It typically has a longer lifespan
    - Sadly no time left for implementing the two tokens solutions:
      - Temporary solution: access tokens are longer valid
  - Exploring technologies like keycloak, OAuth2, ... for two token solution
  - Password should only be transmitted by using https
    - In that case it can be transmitted as clear text

## 2. Technical documentation <a name="technical"></a>

### 2.1 CI/CD & DevOps <a name="devops"></a>

_Author_: Felix

Every Project, which should be deployed for its own, resides on its own Azure
DevOps Project. Those Azure DevOps Projects are holding at least two
Repositories. The first Repository holds the Project itself, the second Repository
holds the deployment definition:

```
Azure DevOps Projects/
├─ DataCollector/
│   ├─ DataCollector/
│   │   ├─ project files etc.
│   │   ├─ Dockerfile
│   │   ├─ azure-pipeline.yaml      -> builds & pushes Dockerimage
│   ├─ Timescale/
│   │   ├─ Dockerfile
│   │   ├─ azure-pipeline.yaml      -> builds & pushes Dockerimage
│   ├─ DataCollector.deployment/
│   │   ├─ docker-compose.yaml      -> references Images & defines Deplyoment
│   │   ├─ azure-pipeline.yaml      -> docker compose up
```

If one project builds up of multiple separate Components (eg. Application &
Database), there are Repositories for every Component. Every Component has its
own Pipeline, which builds a docker image and pushes that to a Container
Registry.

The deployment resides in its own repository. In this way, it is possible to
have the deployment definition under version control as well.

### 2.2 Authentication <a name="auth"></a>

_Author_: Bianca

JWT Bearer Tokens are used to authenticate the end user. The front-end application therefore sends an authentication request to the server when logging in, in which the e-mail address and password are transmitted. The password is transmitted in plain text and hashed on the server side. The password is hashed using "PBKDF2WithHmacSHA512" (PBKDF2: Password-Based Key Derivation Function 2). Specifically, an HMAC is calculated using the SHA512 hash function. A secret is also taken into account in the calculation.

The system then checks whether a corresponding account with the same access data exists. If so, a token with a validity of 15 minutes is generated, signed with the private key and sent back; this token must then be sent with every request. In the form of claims, this token also contains further information about the currently authenticated user, namely their ID and household ID. The user's role is also included. A distinction is made here between admin and user. The difference between the roles is that the admin can see and manage the household data. In addition, only the admin can add/remove household members or rewards and delete the household. The advantage of this is that the IDs do not have to be provided with every request to the server, but can be transferred directly via the token and read out on the server side.

Access to all REST methods is controlled via annotations. This specifies which methods are permitted for which roles. In all methods that require authentication, the system first checks whether a JWT token is available and contains the necessary claims. In addition, the e-mail address of the user within the token is compared with the e-mail address from the security context to ensure that the user who is currently logged in is also the user for whom the token was issued. Quarkus verifies the JWT Token automatically, so no manual check is needed for this.

The main problem with bearer tokens is that they can no longer be revoked. Normally, they only become invalid once the specified validity period has expired.
Ideally, OpenIDConnect would therefore be used in production, which is based on OAuth2 and can currently be described as the standard for this use case. It could also be used to offer the user the option of logging in via their Google or Facebook account, for which the user would only need to remember one password. But the main advantage is that OpenIDConnect distinguishes between access and refresh token, so the user only has to log in once and one can flexibly handle the period of validity. However, as a separate identity provider would have to be provided for the implementation of OpenIDConnect, the JWT tokens were preferred for this demo project.

### 2.3 Embedded Device <a name="embedded"></a>

_Author_: Lucas

The embedded device builds the bridge between the smartmeter and the server. Its two main functions are reading the smartmeter data and transmitting it to the server. Goals for the embbeded device were for it to be independed of local infrastructure like wifi or external power.

#### 2.3.1 MCU <a name="mcu"></a>

The esp32-c3 was choosen as the main computing unit for the embedded device. The main reason for this is the onboard jtag debugger, its cryptrographic features and the low price. Besides this every other microcontroller could be choosen.
An USB-C connector is used to interface the jtag debugger and deliver current to the esp.

#### 2.3.2 MBus <a name="mbus"></a>

The kaifa MA309M smartmeter transmits its data every 5 seconds over and MBus interface. A onsemi NCN5150 is used to translate the signals to 3.3V UART signals with the form 2400Baud, 1 Stopbit and even parity.
The smartmeter can deliver up to 24mA over the two MBus wires. The NCN5150 utilises an internal 3.3V - 15mA regulator to deliver the current to a mcu. In the early design and prototype stage this voltage source was used to power the esp32-c3 and the nb-iot modem and was connected in parallel to the usb power.
This connection lead to repeating errors in the uart transmission. The following image shows the corrupted data in comparission to correct data.

![MBus Error](/images/MBus_error.jpg) _Figure 1: MBus Uart data error_

The image reveals errors in every few bytes. This has been confirmed through the use of an oscilloscope. Additionally, the analysis indicates a consistent number of flipped bits, suggesting a potential issue with the internal power regulator of the NCN5150. Upon disconnecting the 3.3V voltage supply from the NCN5150, connected to the esp32-c3 and nb-iot modem, the problem was resolved. It is likely that the support capacitor discharged too rapidly, causing the voltage regulator to fail.

The smartmeter data is packed in four layers. The two lowest ones are the mbus hardware and link layer. The link layer is extended by the "source address" and "destination address" bytes. The mbus payload contains a DLMS/Cosem Frame which is split onto two mbus frames. The DLMS/Cosem payload is encryped with AES-GCM using a 16byte key and the title concatinated with the framecounter as the start-vector. The decrypted payload contains the data in the format of a DLMS "Data Notification" Frame. Since there is no documentation for the exact format the data is parsed by scanning for the OBIS codes and reading the following bytes.

![MBus Data Format](/images/MBus_frame_format.png) _Figure 2: MBus Frame format_

The frame is parsed from the incomping uart data by scanning for the start (0x68) and length bytes and than reading the rest of the frame depending on the value of the length byte. The address byte is allways 0xFF for this smartmeter showing that the frame is a broadcast and doenst requiere a response. The control-information byte can be 00, 01, 10 or 11 and shows if the paylaod is fractured onto multiple frames with a maximum of 4 frames (0x11).

The relevant parts of the DLMS header are the Title, APDU Length field and the Framecounter. The Framecounter and Title are concatinated and form the star-vector for the decryption. The APDU length field give information about the lenght of the encrypted payload. It can be 1, 2 or 3 Bytes long depending on the first byte. is Its value 0x80 or smaller its the paylaod length, is its exactly 0x81 the next byte is used as paylaodf length and if its 0x82 the next two bytes are interpreted as uint16 payload length.

#### 2.3.3 Power study <a name="power"></a>

The original goal of for the embedded device was to be completly independed of the local infrastructure. For that it should receive its power over the mbus connections from the smartmeter. As described before the errors occured while powering the esp32-c3 and nb-iot modem from the mbus transceiver. To validate the speculation that the device needs to much current a power study was performed.

In Figure 3, the power consumption of the embedded device is shown. The supply voltage of 3.3V is turned on around second 1. This initiates the boot process of the ESP32-C3 and the SIM7020E modules. In the time frame of seconds 7-9, the module establishes the mobile network connection. Subsequently, the connection to the server is established, and the authentication takes place. All M-bus messages received up to authentication are then sent to the server. Afterward, the module sends the received M-bus message to the server every 5 seconds. The current is measured using the HMC8015 and read and logged using a Python script. The baseline consumption of the ESP and SIM7020E together is approximately 25mA. Since the M-bus transceiver NCN5150 can only deliver a maximum of 15mA, the ESP32-C3 and SIM7020E must be externally powered. As the average current consumption is around 32mA and an average Li-Ion battery with 2.7Ah lasts only about 84 hours, this option is also ruled out. Theirfor the goal of a completly independent embedded device has failed and it needs to be powered using an external powersupply.

![Embedded Device power study](/images/current_study.png) _Figure 3: Embedded Device power study_

The meter key needed to decode the data received from the smartmeter is read from the flash at the start of the programm. Is the key not available yet because the user has to enter his personal key, an encrypted key request is send to the server. The server will respond with the encrypted key and the meter data can be received and decryped.

#### 2.3.4 NB-Iot Modul <a name="nb-iot"></a>

NB-IoT was selected for data transmission between the embedded device and the server. The primary rationale behind this choice is to ensure that the embedded device remains independent of local infrastructure, such as WiFi. NB-IoT, in particular, was chosen for its excellent wall penetration and reception capabilities. Unlike alternatives like LoRa or Zigbee, NB-IoT eliminates the need for a gateway that may not be universally applicable in every household.

The SIMCOM SIM7020E Module was chosen to expedite development, leveraging existing knowledge from previous projects. This module interfaces with the ESP32-C3 using a logic-level shifter for the UART lanes. A custom C driver, utilizing the ESP-IDF, was developed for the SIM7020E module. The driver can be easily migrated to other microcontrollers by modifying the UART and GPIO functions in two specific functions, minimizing the required effort.

The driver initializes the ESP32-C3 UART and attempts to communicate with the modem. If the modem fails to respond, the ESP triggers the power key to reset the module. Upon successful communication, the driver initiates the network connection.

#### 2.3.5 Firmware <a name="firmware"></a>

![Firmware](/images/Embedded_Device_Firmware_Overview.png) _Figure 4: Firmware_

#### 2.3.6 Micro Guard UDP <a name="mgudp"></a>

Micro Guard UDP is a specifically developed transmission protocol that enables secure data transfer over UDP. It is designed to fully support the three security goals of confidentiality, integrity, and availability while optimizing the amount of data.

Confidentiality is ensured through Challenge-Response authentication of the client to the server. The client sends its ID to the server, and the server responds with a 16-byte random number. The client generates the session key from the random number using HMAC and a private key. Subsequently, the random number and the session key are hashed and sent back to the server. The server also generates the session key and hash in the same way to compare with the client's response. If both values match, the client is authenticated on the server, and the server sends an authentication confirmation to the client. To ensure the confidentiality of this response, it is formed by concatenating it with the authentication status using HMAC and the session key derived from the random number. This process is also used for the server's responses to data frames, with the difference that the frame counter is used instead of the random number.

To ensure data integrity, the data is encrypted using AES_GCM. The counter required for this purpose is used as the frame counter and starts at the random number, increasing by the random number % 100 with each data frame.

Availability is achieved through server responses to each frame. To prevent Deauth attacks, the response is concatenated with the current frame counter, and an HMAC is created with the session key. Messages deviating from the HMAC of a potential server response are discarded by the client.

The security of the protocol relies on the private key generated on the embedded device, which must be securely present on the server and linked to the ID.

To achieve small data sizes, metadata is largely omitted. The only exception is the protocol version, which is used to determine the data format. The hashes Server_Response, during authentication, as well as Data_Response, are each truncated to 4 bytes to reduce the amount of data. Thus, the protocol is well-suited for NB-IoT applications.

![Micro Guard UDP](/images/mgudp.png) _Figure 5: Micro Guard UDP_

The server side is implemented in python ontop of udp sockets. The server contains the client state with the ip, port, client key, id, session key and framecounter. Whenever a udp packet arrives at the server a new thread is started to handle the packet. The client state gets choosen by the ip and port. If a packet with a new ip and port arrives the authentication is started. Packets of unauthenticated clients will be ignored.

#### 2.3.6 Security <a name="security"></a>

To ensure the security of the private key stored on the embedded device, it is generated once and stored in the Digital Signature Module of the ESP32-C3. Consequently, the key is not externally accessible, and the session key derivation can be directly executed within the Digital Signature module.
Due to issues encountered with storing the key in the Digital Signature Module in the current prototype, the key is currently stored in the Non-Volatile Storage (NVS) of the ESP.

#### 2.3.7 Hardware design <a name="hardware"></a>

The design is fairly simple containing only 3 major components:

- MCU: ESP32-C3
  - USB-C Connector
  - LDO: AMS1117
- MBus Trancsceiver: NCN5150
- NB-IoT Modem: SIM7020E
  - Micro Sim Adapter: HYC240-SIM06-150
  - Level Shifter: TXB0104

![Schematic](/images/Schematic.png) _Figure 6: Schematic_

![Layout](/images/Layout.png) _Figure 7: Layout_

### 2.4 Testing <a name="testing"></a>

_Author_: Bianca

"Householdmanagement" and "Accountmanagement" have been tested using JUnit5.

#### 2.4.1 Accountmanagement

Figure 8 shows the testing report of the Accountmanagement service. As we can see, 176 tests have been written for this service.

![Testing Report - Accountmanagement](/images/test-summary_accountmanagement.png) _Figure 8: Testing Report - Accountmanagement_

In Figure 9 on the other hand, the Jacoco report of the Accountmanagement service can be seen. This report shows the code coverage of the tests.
![Jacoco Report - Accountmanagement](/images/jacoco-report_accountmanagement.png) _Figure 9: Jacoco Report - Accountmanagement_

#### 2.4.2 Householdmanagement

In Figure 10 the testing report of the Householdmanagement service is shown. For this service, 134 tests have been written.
![Testing Report - Householdmanagement](/images/test-summary_householdmanagement.png) _Figure 10: Testing Report - Householdmanagement_

Figure 11 illustrates the code coverage achieved by those tests.
![Jacoco Report - Householdmanagement](/images/jacoco-report_householdmanagement.png) _Figure 11: Jacoco Report - Householdmanagement_

### 2.5 DataCollector: Database First <a name="datacollector"></a>

_Author:_ Dominik

To store the data of our different microservices, we used a relational database for each service. Accountmanagement and Householdmanagement use a normal PostgreSQL database to save their data.

Our DataCollector, on the other hand, is the core-service for storing and transforming our time-series measurement data. As working with time-series data can be complicated and some operations very resource-intensive, we had very different requirements for the database.

The database should:

- provide native support for time-series data
- provide various optimized methods to aggregate time-series data
- to work efficiently on searching time-series data
- not be too different from our other databases

That is why, instead of using a normal PostgreSQL database, we used a specialized time-series database called _TimescaleDB_ to fulfill these requirements. TimescaleDB is a relational database that extends PostgreSQL with specialized time-series support. It provides:

- native time-series support
- hyper-tables that partition data based on time-data
- advanced time-series functions
- (horizontal) scalability

As earlier mentioned, it was a crucial goal to have a performant way of retrieving and storing time-series data. That is why, we focused on a database-first approach when implementing the database for the DataCollector, i.e. we focused on modelling the database model first with performance in mind and only later wrote the code for it.

In the following we will

- delve into the database schema
- explore how we made it better accessible for the application via database views
- and lastly how trigger functions helped us mapping to map between the database model and the views as well as to handle errors on a database side.

#### 2.5.1 Database Schema

The database schema for the DataCollector looks as following:

![Database Schema - DataCollector](/images/data_collector_schema.png) _Figure 12: Database Schema - DataCollector_

- "Measurement" is our key table. It stores the time-series data for various smart meter measurements. Primary key is the reading time alongside the (meter)device_id. It is also a hyper-table with the partition_column "device_id", i.e. we partitioned the time-data alongside our (meter)device_id for a better performance.
- "Devicecategory" stores the various device categories that we use for labelling measurement data in the tagging table. Primary key is the category_name.
- "Device stores" the concrete devices inside a household (from the household service). It is used as a sort of individual naming for tagging measurements. Primary key is the device_name as well as the (meter)device_id, i.e. per meter device / household there can only be one device called like that.
- "Averagepriceperwh" stores the pricing plan associated with a meterDevice (for calculating electricity costs later on). Primary key is the (meter)device_id and start_date (of the pricing plan).
- "Tag" is our second key table. It stores the tagging information for measurements. It does so by having a reading_timerange in which all measurements from the same (meter)device_id all into. Primary key are the reading_timerange, the device_category_name, the (meter)device_id and the device_name. To prevent having overlapping timeranges, a tag_exclusion_constraint checks if the timeranges for the same primary key overlap. If yes, a new tag cannot be inserted.

#### 2.5.2 Views

As seen above, tag and measurement are not directly referencing themselves. This is because hyper-tables are not allowed to have references on foreign keys. As manually assembling measurements and tags would complicate the application's code a lot, a solution was to use so-called database views.

A database view is a virtual table derived from the data in one or more underlying tables, presenting a specific perspective or subset of the data without storing it physically, often used for simplified or customized data access. It is also possible to define INSTEAD OF triggers on these views (more about that later).

Now, in order to simplify the access to measurements and associated tags, we defined the measurement_w_t (read "with tag") view. It is defined as following (see figure 13):

![Measurement View Definition](/images/measurement_w_t_view_deifnition.png) _Figure 13: measurement_w_t View Definition_

As you can see, for the view we assemble measurements with their associated tags by a join and put these tags into an array aggregate. With that, they seem as one common table to the user.

We also defined a view for the tags that map the tsrange to two separate timestamps (easier to read for the backend).

Both the tag view and measurement_w_t view serve as an access point for the backend to the measurement and tag tables. This also means, the measurement and tag tables are not intended for being directly accessed by the backend.

#### 2.5.3 Trigger Functions and Exception Handling

Now, when trying to perform persist or update on the view inside the backend, the database does not inherently know how it can disassemble the view data back into their respective tables. For that we need to define own INSTEAD OF trigger functions that, like the name suggests, are being executed instead of a certain function.

We implemented these functions on

- the insert of tag_view
- insert and update of the measurement_w_t

There we disassemble the views into their original parts and persist the changes into their respective tables.

These trigger functions can also help us to lever error handling from the backend into the database system. For example, timescale is very mighty with processing time-series data. As earlier mentioned, we need to check that the reading_timerange of the tags are not overlapping. Now, instead of performing overcomplicated check in the backend, we can make use of our exclusion constraint and catch the error inside the trigger function. Now we can perform a repair step where we "fuse" the overlapping tags (see image...).

![Trigger: Exception Handling for Overlapping TimeRanges](/images/trigger_exception_handling.png) \_Figure 14: Example for handling exceptions in a trigger function (for overlapping time ranges)

#### 2.5.4 Array User Types

As earlier mentioned, we tried to keep the backend free from overcomplicated database as much as possible. Sadly, to this date, Hibernate does not support array types natively, so for mapping the array aggreate inside the measurement_w_t view, we had to use so called UserTypes inside Hibernate.

In Hibernate, a User Type, also known as a custom or user-defined type, is a mechanism that allows you to define and use custom data types in your entity classes. A User Type lets you map a custom Java type to a corresponding column type in the database.

Inside our self-defined TagUserType, we now defined what should happen when we read the tags array aggregate from the database (nullSafeGet) and what should happen when write back to the database (nullSafeSet). Inside these functions we map the data into their respective form for both the database and backend.

### 2.6 Exploratory Data Analysis <a name="eda"></a>

_Author:_ Dominik

One of our original goals of the project was to explore if we can train useful models from collecting and tagging our measurement data. For that purpose we did two EDAs on our data.

In a first step we analyzed the raw measurement data (provided by lecturer Peter Reiter). Observations made in that EDA were that:

- current lanes and instanteousActivePowerPlusW are highly correlated,
- voltage does not give us a lot of information,
- most information can be received from the current and power attributes
- there is a pattern of peaks throughout the day
- monthly energy use varies a lot but needs more data to be analyzed

In a second step we had a short session where we were trying out different devices and label the data to these devices. As the experiment was only very short, we only had few data to analyze. Still, we were able to make some observations:

- current behavior between devices is unique
- some devices have a fluctuating current consumption, while others have constant current consumption

### 2.7 Communication between microservices <a name="communication"></a>

_Author_: Bianca

The Frontend communicates directly with the Backend by sending requests to REST interface. The Backend microservices on the other hand communicate by events using a Publish-Subscribe approach. So, a microservice can publish events on a specific topic and every microservice which is interested in this topic subscribes it. For publishing and subscribing events, we used Redis Streams.
The consumer of the event then handles the event by e.g. making changes and saving relevant data in its own database.

So, for example, if a household member is edited, the Account-Management publishes an event containing relevant data, e.g. id of the household the member belongs to, id of the member itself as well as the edited data of the member which is relevant for the Household-Management service. The Household-Management on the other hand consumes this event, retrieves the corresponding household and edits the member according to the data in the event. It then saves the updated member and now has its own representation of the member, which is in sync with the original representation which resides in the Account-Management service.

## 3. Project progress report <a name="projectprogress"></a>

### 3.1. Sprint 0 (14. - 27. September 2023) <a name="sprint0"></a>

_Author_: Bianca

_Team_:
Defined **target group** (end users) to be private households with very little technical knowledge, which might not have WLAN reception in the meter room (e.g. basement).

For this, we created a proto persona which is displayed in Figure 15.

![Proto persona](/images/ProtoPersona.png) _Figure 15: Proto Persona_

Additonally, following main use cases were defined:

- Power consumption monitoring
- Power saving hints
- Device identification in form of labeling

---

### 3.2. Sprint 1 (28. September - 12. Oktober 2023) <a name="sprint1"></a>

_Author_: Bianca

_Team_: Application scenarios in form of **user stories** were defined.

_Security_:

- As a user, I want to be sure that my data is stored pseudonymously and transferred securely so that I don't have to worry about possible data theft.
- As a provider, I want to release the AI models only if the user has agreed to release his or her data anonymized to the Open Data Platform API in order to create incentives for data sharing.

_Account Management_:

- As a user, I want to be able to create a household and assign an electricity meter to see the electricity consumption for each of my households.
- As a user, I want to associate a device when I register my account so that I can later collect data for my account.
- As a household, I want to create different profiles for my household with demographic information (age and gender) to assign the different consumptions to the different profiles.
- As a user, I would like to have an application where I can optionally enter my demographic information, such as household size and age of household members, to see more precise comparative values with similar households, if I want to.
- As a user, I would like the ability to delete my account and the data collected, or to decouple the data from the profile.

_Power consumption monitoring/power saving_:

- As a user, I want to be able to see my power consumption (in relative real time) and over the day, so I can estimate what times I needed how much power.
- As a user I would like to set power saving goals and get a report at the end of a set interval if I was able to meet them, to keep an eye on my progress.
- As a user, I would like to receive recommendations for electricity tariffs based on my consumption in order to find the best tariff for me personally and thus save money.
- As a user, I would like to see how much electricity I use on average per day or week so that I have a quick overview of my personal consumption.
- As a user, I would like to see at a glance how my electricity consumption has changed over the last few days/weeks/months, so that I can see whether I tend to save electricity or not.
- As a user I would like to see my current consumption translated into the electricity costs for the last 5 / 10 / 20 / ... days (selectable) as a graph on the start page to see if my electricity saving measures are working.
- As a user I would like to get a green info or red warning on the start page, if I could reduce my power consumption in the last 30 days on average, to be motivated to save power when opening the app regularly.
  Display of consumption: graphical display of consumed power, possibly annotated with estimated consumption per time with the possibility to correct it.
- As a user, I would like to receive my power savings target report as a push notification and be able to deactivate it in the settings to be reminded of it if I haven't opened the app for a while.

_Device identification and labeling_:

- As a user, I would like to be notified to label power peaks so that the labels help the evaluation later.
- As a provider, I would like a family leaderboard for labeling to see who labels the most each week to create incentives for labeling.

- As a household manager, I would like to be able to manually set a reward for the leaderboards (free text such as "free pizza", not having to vacuum, ...) to motivate my household to label.
- As a user I would like to have meaningful peak periods pre-selected for labeling to reduce the effort for me.
- As a user, I would like the application to automatically distinguish between my frequently used devices, so that I can better estimate how much power my respective devices require.
- As a user I would like to see meaningful labels suggested for peaks based on historical data (e.g. washing machine has a peak of 500 watts).
- As a user I would like to see which device needs the most power on average, so that I can save money by using a device with higher energy efficiency instead or by starting the respective device only if necessary.
- As a user, I want to know which devices use how much power on average so that I can initiate changes in consumption to save money if necessary.
- As a user, I want to know when a more energy efficient appliance will pay for itself.

**Optional use cases**

Additionally, we defined problem detection and comparison with demographically similar households to be interesting use cases, but decided to implement them only if we have enough time.

_Problem and danger detection_:

- As a user, I would like to be notified if an unusually high power consumption has been measured over a longer period of time, so that I can become aware of a possibly defective device.
- As a user, I would like to be informed about possible fire hazards in my power grid so that I can act in time.

_Comparison with demographically similar households_:

- As a user, I would like to be able to compare my personal usage data with comparable households so that I can better assess whether I am using more or less electricity compared to demographically similar households. (Show average value of households as a line and indicate from how many different households this average was calculated).
- As a user, I would like to be able to specify for a household where this household is located, so that the electricity consumption there can be put into relation with the local circumstances.

**Individual responsibilities**:

- _Embedded device_ (implemented by Lucas):
  select possible hardware components, create schematic and layout for embedded device, order necessary components and assemble board prototype

- _Test data_ (implemented by Lucas):
  Setup test data simulator

- _MQTT Setup_ (implemented by Lucas):
  Setup mqtt broker on server

- _Design of application_ (implemented by Bianca):
  Style guide, paper prototype, blueprint

- _Database_ (implemented by Dominik):
  Setup timescale and familiarize with it

- _Deployment environment_ (implemented by Felix):
  Setup Azure pipeline and local deployment

---

### 3.3. Sprint 2 (13. Oktober - 2. November 2023) <a name="sprint2"></a>

_Author_: Bianca

**Team**: Define database schema, define microservices & systems architecture, system operations and collaborators

Figure 13 shows the architecture we planned to implement for this project. Everything with dashed lines was not planned to implement, but can be seen as a possible extension for this project.

![Architecture](/images/architecture.png) _Figure 16: Architecture_

Following figure 14 shows the database schema we came up with in this sprint.

![Database scheme](/images/database-scheme.png) _Figure 17: Database schema_

**Individual responsibilities**:

- _Embedded_ (implemented by Lucas):
  Define Mbus protocol, test hardware and resolve esp flashing problems

- _Database & Data Analysis_ (implemented by Dominik):
  Familiarize with Timescale, setup necessary tables, first exploratory data analysis on test data

- _Organizational stuff & MQTT Broker_:
  Documentation GitHub pages, time schedule, setup projects and repositories, adjustment of paper prototype (implemented by Bianca)
  User stories in Azure, MQTT broker setup, (implemented by Felix)

---

### 3.4. Sprint 3 (03. November - 16. November 2023) <a name="sprint3"></a>

_Author_: Bianca, Dominik

**Team**: Smallrye mapping

**Individual responsibilities**:

- _Embedded_ (implemented by Lucas):
  Data decoding, M-Bus software implementation, error handling of hardware implementation

- _Deployment_ (implemented by Felix):
  Deploy simulator, database and the other projects

- _Application FrontEnd_:
  Export of figma style & adjustment for real application / UI (implemented by Bianca), implementation of tagging view (implemented by Dominik)

---

### 3.5. Sprint 4 (20. November - 07. December 2023) <a name="sprint4"></a>

_Author_: Dominik, Lucas

- _Embedded_ (implemented by Lucas):
  Modem driver implementation, esp-idf mqtt<->modem interface evaluation
- _Application BackEnd_:
  Entering test data into productive system, implementation of API for measurements for a given time period, API for grouping and averaging of measurements (implemented by Dominik), implementation of account service (login, register new household, retrieving, editing and saving user, retrieving, editing and saving household data, deleting household and user, retrieve/ add/ delete/ edit household member, add email address to household member), complete authentication with jwt tokens (implemented by Bianca)
- _Application FrontEnd_:
  Finalizing frontend-backend connection for tagging (implemented by Dominik), implementing frontend for account service (login, register new household, retrieving, editing and saving user, retrieving, editing and saving household data, deleting household and user, retrieve/ add/ delete/ edit household member, add email address to household member, authentication functionality) (implemented by Bianca), role-based view (implemented by Felix)

---

### 3.6. Sprint 5 (10. December - 20. December 2023) <a name="sprint5"></a>

_Author_: Lucas, Felix

- _Embedded_ (implemented by Lucas):
  MicroGuardUDP protocol definition and implementation on embedded device and server
- _Deployment_ (Felix): building Services as Dockerimages in Azure CI Pipeline & pushing them to ghcr.io. Pull images to Docker Host and start them, orchestrated in Docker compose. Triggered by Azure CD Pipeline.
- _Labelling Data_ (Dominik): Measurement data, provided from embedded device and persisted in Database, needs to be labelled by Users according to their energy usage. Measurement Data that belongs to a time period, needs to be annotated with Data about Devicecategory and Devicenames, that ran during that period and caused energy consumption.
- _Measurement View_ (Dominik): Measurements need to be retreived from database and mapped to useable format. Frontend has to fetch that data and present that to user.
- _Data retrieval from Embedded Device_ (Lucas, Dominik): Data needs to be retrieved from MQTT-Broker, mapped to useable format and persisted in Database. Because our embedded device sends data every five seconds but skips if measurement does not differ from previous measurement (save data transfer), our backend needs to auto-fill the missing gaps
- _Error handling_ (Bianca): In case of errors that occur in backend, the frontend needs to stay functional and useable
- _logout_ (Bianca): logged in users need to revoke their login when they want to end their session
- _create installable PWA_ (Bianca): Frontend can be installed as PWA when manifest exists
- _Incentives_ (Bianca): users want to add and edit incentives, so that users can be motivated for labelling data
- _Responsive Frontend_ (Bianca): Frontend needs to be responsive, so it can adapt to different devices and different screensizes

---

### 3.7. Sprint 6 (22. December 2023 - 9. January 2024) <a name="sprint6"></a>

_Author_: Dominik, Lucas

- _Streaming-Infrastructure_:
  Defining AccountUpdated & TaggingCreated events, setting up redis publishing infrastructure, implementation of AccountUpdatedEvent + TaggingCreatedEvent (implemented by Felix, Bianca, Dominik)
- _Embedded_ (implemented by Lucas):
  Technical documentation, MicroGuardUDP Client reworked to act as the Simulator with the Test data
- _Application BackEnd_:
  Finalizing measurement database view by adjusting data collector repository, implementation of api for accumulated measurements for a given time period and bin number (implemented by Dominik),
  implementation of retrieving, adding and editing saving target, leaderboard, unit and integration tests for account-management and household-management, OpenAPI definition for account-management and household-management, consumer for TaggingCreatedEvent (implemented by Bianca)
- _Application FrontEnd_:
  Implementation of role-based rendering (implemented by Felix), view for retrieving, adding and editing saving target, view for leaderboard, and form validation (implemented by Bianca)

---

### 3.8. Sprint 7 (12. January 2024 - 24. January 2024) <a name="sprint7"></a>

_Author_: Dominik

-  _Embedded_: implementation of meta key requests (implemented by Lucas)
- _Application BackEnd_: implementation of device management (create, update, delete) in data collector and household service (implemented by Bianca and Dominik), synchronization of consumer and publisher events (implemented by Bianca and Dominik), refactoring account and household management (implemented by Bianca), refactoring data model for data collector (implemented by Dominik)
- _Application FrontEnd_: implementation of energy costs view (implemented by Dominik), device management view (implemented by Bianca), energy consumption view (implemented by Felix)
- _Deployment_ (implemented by Felix): creation of docker network to connect all microservices, reverse proxy (for preventing ui cors problems)
- _Testing_: tests for device management (implemented by Bianca), application and integration tests for datacollector (implemented by Dominik)
- _Data Analysis_ (implemented by Dominik): second exploratory data analysis on tagged data

---

### 4. Future work <a name="futurework"></a>

_Author_: Bianca

Due to the limited time, we did not implement every functionality we initially planned. So, following could be considered as **possible extensions** of this project:

- Identify most energy efficient devices in comparison to comparable devices of other households (devices-overview page)
- Calculate energy efficiency of a specific device
- Show trend of costs for a day/week/month/year (energy-saving page)
- Generate an energy saving report and send it as a PDF file to the interested customer
- Include more energy suppliers than only VKW
- Use actual pricing plans of the corresponding supplier instead of hardcoded demo data
- Train a machine learning model to recommend a pricing plan based on the customers energy consumption
- Make predictions of energy consumption, e.g. for a month/ year

Also, the applications could be **optimized**. For example, in the account-management service, because of the user and jwt validation, all methods of the controllers have some lines of code which are identical. Authentication in general could be extracted into an API-gateway.

The smartmeter-ui application on the other hand, has yet to be tested. Additionally, there is some code duplication regarding error handling, which could be optimized.

Furthermore, as described in chapter 2.2 Authentication, OpenIDConnect could be implemented.

_Auther_: Lucas

Three points are missing regarding the embedded device. First of all, a long-term function test is required. The embedded device has been tested by transmitting data read from a smart meter for around 10 minutes, during which no data was lost. To validate the entire functionality, the embedded device needs to be tested on a smart meter in a real or realistic household for a more extended period. This way, the successful transmission of all data points and the filter function can be verified. Problems that could occur during the field tests include:

- Reboot of the embedded device due to unknown runtime issues like buffer overflow, leading to lost data or a complete system shutdown.
- Loss of network connection and inability to reconnect, resulting in lost data.
- Overflow of the data storage queue due to too many retransmission tries, leading to lost data.
- Loss of MicroGuardUDP Server connection, causing data loss.

The next point is the over-the-air update functionality, which was stated as a requirement at the start. OTA updates are necessary to deploy software updates and critical patches to the devices while they are in the field. Since the OTA update should also be tested, it should be implemented before the tests or the tests need to be repeated afterward. The OTA update could be implemented by adding a file download functionality to the MicroGuardUDP protocol or by using a separate TCP implementation like HTTP. Since the updates should not occur too often, data usage won't be critical. The Espressif framework has a built-in API to exchange the firmware running on the ESP32-C3, making it fairly straightforward to implement this part.

The third and last thing that should be done in the future is a complete redesign of the hardware to use less current. As shown in the chapter [Embedded Device: Power Study](#power), the average current used by the embedded device is too high to run autonomously. The current usage can probably be greatly reduced by using a modem with an embedded MPU, eliminating the need for the logic level shifter between the ESP32-C3 and the SIM7020E. A reference design using the Adrastea-I modem is shown below. Another change that needs to be made is using the NCN5150 in the QFN20 package. The package used in the current version doesn't support more than 2 Unit Loads, but to get the maximum current of 15mA, 6 Unit loads are needed.

![Schematic](/images/Schematic_Adrastea.png) _Figure 13: Schematic using the Adrastea Modem_

![Layout](/images/Layout_Adrastea.png) _Figure 14: Layout using the Adrastea Modem_

---
