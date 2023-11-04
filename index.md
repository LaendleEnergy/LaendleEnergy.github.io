# LaendleEnergy Documentation

Created: 21.09.23 by Bianca

Updated: 14.10.23 by Bianca

## Table of contents
1. [Coaching documentations](#coachings) \
1.1. [Project coaching (Wolfgang Auer)](#auer) \
1.2. [Data Management (Peter Reiter)](#peter) \
1.3. [Advanced data analytics (Sebastian Hegenbart)](#sebastian) \
1.4. [UI (Walter Ritter)](#walter) \
1.5. [Web (Daniel Rotter)](#daniel) \
1.6. [Security (Armin Simma)](#armin) 
2. [Technical documentation](#technical) \
2.1 [CI/CD & DevOps](#devops)
3. [Project progress report](#projectprogress) \
3.1. [Sprint 0](#sprint0) \
3.2. [Sprint 1](#sprint1) \
3.3. [Sprint 2](#sprint2) \
3.4. [Sprint 3](#sprint3) \
3.5. [Sprint 4](#sprint4) 

## 1. Coaching documentations <a name="coachings"></a>

### 1.1. Project coaching (Wolfgang Auer) <a name="auer"></a>

---
**Project idea** (21.09.23)

Further procedure:
- Define application scenarios
- Define requirements
- Define target group (Who are we doing this for?)
- Create 1-2 proto-personas
- Create paper prototype
--- 

**User stories** (12.10.23)

*Proposal*: The original idea was to set sliders on an energy consumption curve to indicate which device was used in a particular curve. Recommended was to do this the other way around, so to first start a device and then record its usage so that the system immediately learns whihc device has which consumption. This procedure is recommended because it might make more sense in terms of usability, but it does not have to be implemented this way.

*Recommended further use cases*:
- Notes on the energy efficiency of the appliances in question: e.g., "Your hair dryer uses x kWh, which is y kWh more than the average hair dryer needs" or "If you replace this appliance with this appliance, you could save yourself x kWh"
- Problem detection based on the collected data for a device would add some practical value to the application. One example for this would be to monitor energy consumption of a washing machine. If it uses more electricity then usual, it might be calcified.
- Suggestions on when to turn on which devices, like e.g. "PV system brings x electricity right now so it would be reasonable to turn on applicance y now", or turn on washing machine in 3h.
---

### 1.2. Data management (Peter Reiter) <a name="peter"></a>

### 1.3. Advanced data analytics (Sebastian Hegenbart) <a name="sebastian"></a>

### 1.4. UI (Walter Ritter) <a name="walter"></a>

---
**Paper Prototype** (31.10.2023)

- Button placement guide (?)
  o Define primary / secondary buttons
  o "Third" unimportant subordinate buttons only as text links
- Home page:
  o Information / benefits, what the application does, why you should use the application
  o Designed as a landing page:
   Problem description
   Problem solution
   1 sentence is sufficient
- Registration page:
  o Password criteria? None defined so far
  o (Username must be unique) -> Name instead of username. When is the check carried out?
  o If the user name is used, suggest another user name when clicking on it?
- Electricity tariff selection page:
  o Step display, where you are in the registration process
  o Electricity provider dropdown (initially only with vkw)
  o Introduce a meter number check to see if it already exists
- Home page:
  o Introduce comparison diagram (how much have I saved / consumed more in comparison)
  o Pay attention to axis descriptions
  o Use terminology of target persons
  o Arrangement of diagrams: Display important diagrams first, e.g. if saving electricity is important, display electricity costs and savings first
  o Create diagram Process: Define target. Which metrics are available to me for the target. Define exactly what the diagram should show -> Implementation via Gold Query Metrics model.
- Labeling page:
  o Label name: "Assign consumption"
  o Color the curve / area when selecting the time period so that consumption is easier to see
  o Revise the time axis- Competition page
  o Edit button should only be visible to the admin
  o In the ranking list, write how often you have labeled
  o Create a button for labeling from the ranking list
  o Highlight benefit for labeling
  o Overview dialog for the missing first place such as "Hey Walter, assign 5 more labels to get the pizza 😀"
  o Rename navigation button: instead of "Comparison" something else, e.g. "Household members ranking", "Reward"?
- Personal information page
  o Save highly sensitive data securelyo Mark optional fields
  o Mark page explicitly as optional implementation
  o Add an additional password field for repetition
  o Delete account stands out too much as a subordinate button Perhaps only as a text link with color coding (no red!)o Group data between account, contact and household data On the same or a separate page?- Current members pageo Highlight optional fieldso Rename to household members instead of just memberso Highlight household members and profile difference- Create your own profileo More information Which household have I been invited to Previously known information Highlight benefit againo "Join household" instead of "Create profile" as a button- Devices pageo Overview of appliances directly on the appliance page
- Energy efficiency diagram
  o Sell this as a benefit when registering
  o Display devices with "catch-up requirements" first
  o Perhaps display several appliances at the same time (e.g. 5 worst performing)
  o Highlight the difference between appliance category(/type) and appliance label
- Electricity saving page
  o Optional: Suggestions for saving electricity
  o Possibly place tariff recommendation under consumption, as it is not really part of the "Save electricity" tab
  o Define whether the focus is on saving electricity or money
  o Report button placement debatable (because it does not only belong to electricity saving)
  o Describe what a report is
- Electricity consumption page:o Alternating presentation of energy and costs 
  o Insert tariff recommendation there
  o Cost diagrams with tariff dropdown
---

### 1.5. Web (Daniel Rotter) <a name="daniel"></a>

---
**Microservices** (02.11.2023)

*Preparation:*
- Own database for Data Preparation Service?
- ML service -> read only to time series database or own DB?
- Does it make sense to split the microservices in the way we did?
- When to use REST/events?

*Documentation:*
- Consideration: Link measurement to the user?
  o Child consumes so and so much power while playing with toy x
  o Answer questions such as: how much more electricity does it need if you have 1-3 children?
- Data Collector & Data Preparation very closely coupled (Data Preparation always accesses Data Collector), possibly ML service later too
  o Data Preparation Service should not always fetch all data from DB -> becomes a lot if you get new data e.g. every 5 seconds
  o Suggestion: merge (if synchronous) or use CQRS (asynchronous, create new view)  Advantage of CQRS: if Data Collector is down, something can still be displayed)
  o Data Preparation Service should aggregate data immediately  Note: See what Timescale can do, not in the frontend (use hypertables)
  o E.g. also precalculate data for the predefined time periods (annual, monthly, weekly, daily)
  o Build exactly the views for the specified use cases
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



## 2. Technical documentation <a name="technical"></a>

### 2.1 CI/CD & DevOps <a name="devops"></a>
*Author*: Felix

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

If one Project builds up of multiple separate Components (eg. Application &
Database), there are Repositories for every Component. Every Component has its
own Pipeline, which builds a Dockerimage and pushes that to a Container
Registry.

The Deplyoment resides in its own Repository. In this way, it is possible to
have the Deplyomentdefinition under version control aswell.

## 3. Project progress report <a name="projectprogress"></a>

### 3.1. Sprint 0 (14. - 27. September 2023) <a name="sprint0"></a>
*Author*: Bianca

*Team*: 
Defined **target group** (end users) to be private households with very little technical knowledge, which might not have WLAN reception in the meter room (e.g. basement).

For this, we created a proto persona which is displayed in Figure 1.

 ![Proto persona](/images/ProtoPersona.png) *Figure 1: Proto Persona*

Additonally, following main use cases were defined: 
- Power consumption monitoring
- Power saving hints
- Device identification in form of labeling

---

### 3.2. Sprint 1 (28. September - 12. Oktober 2023) <a name="sprint1"></a>
*Author*: Bianca

*Team*: Application scenarios in form of **user stories** were defined.

*Security*:
- As a user, I want to be sure that my data is stored pseudonymously and transferred securely so that I don't have to worry about possible data theft.
- As a provider, I want to release the AI models only if the user has agreed to release his or her data anonymized to the Open Data Platform API in order to create incentives for data sharing.

*Account Management*:
- As a user, I want to be able to create a household and assign an electricity meter to see the electricity consumption for each of my households.
- As a user, I want to associate a device when I register my account so that I can later collect data for my account.
- As a household, I want to create different profiles for my household with demographic information (age and gender) to assign the different consumptions to the different profiles.
- As a user, I would like to have an application where I can optionally enter my demographic information, such as household size and age of household members, to see more precise comparative values with similar households, if I want to.
- As a user, I would like the ability to delete my account and the data collected, or to decouple the data from the profile.

*Power consumption monitoring/power saving*:
- As a user, I want to be able to see my power consumption (in relative real time) and over the day, so I can estimate what times I needed how much power.
- As a user I would like to set power saving goals and get a report at the end of a set interval if I was able to meet them, to keep an eye on my progress.
- As a user, I would like to receive recommendations for electricity tariffs based on my consumption in order to find the best tariff for me personally and thus save money.
- As a user, I would like to see how much electricity I use on average per day or week so that I have a quick overview of my personal consumption. 
- As a user, I would like to see at a glance how my electricity consumption has changed over the last few days/weeks/months, so that I can see whether I tend to save electricity or not. 
- As a user I would like to see my current consumption translated into the electricity costs for the last 5 / 10 / 20 / ... days (selectable) as a graph on the start page to see if my electricity saving measures are working.
- As a user I would like to get a green info or red warning on the start page, if I could reduce my power consumption in the last 30 days on average, to be motivated to save power when opening the app regularly.
Display of consumption: graphical display of consumed power, possibly annotated with estimated consumption per time with the possibility to correct it. 
- As a user, I would like to receive my power savings target report as a push notification and be able to deactivate it in the settings to be reminded of it if I haven't opened the app for a while.

*Device identification and labeling*:
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

*Problem and danger detection*:
- As a user, I would like to be notified if an unusually high power consumption has been measured over a longer period of time, so that I can become aware of a possibly defective device.
- As a user, I would like to be informed about possible fire hazards in my power grid so that I can act in time.

*Comparison with demographically similar households*:
- As a user, I would like to be able to compare my personal usage data with comparable households so that I can better assess whether I am using more or less electricity compared to demographically similar households. (Show average value of households as a line and indicate from how many different households this average was calculated).
- As a user, I would like to be able to specify for a household where this household is located, so that the electricity consumption there can be put into relation with the local circumstances.

**Individual responsibilities**:

- *Embedded device* (implemented by Lucas):
Designed embedded device, order necessary components and assemble board prototype 

- *Test data* (implemented by Lucas):
Setup test data simulator 

- *Design of application* (implemented by Bianca): 
Style guide, paper prototype, blueprint

- *Database* (implemented by Dominik):
Setup timescale and familiarize with it

- *Deployment environment* (implemented by Felix):
Setup Azure pipeline and local deployment

---

### 3.3. Sprint 2 (13. Oktober - 2. November 2023) <a name="sprint2"></a>
*Author*: Bianca

**Team**: Define database scheme, define microservices, system operations and collaborators

**Individual responsibilities**:

- *Embedded* (implemented by Lucas): 
Define Mbus protocol, test hardware

- *Database* (implemented by Dominik):
Familiarize with Timescale, setup necessary tables

- *Organizational stuff*:
Documentation GitHub pages, time schedule, setup projects and repositories (implemented by Bianca)
User stories in Azure, MQTT broker setup, (implemented by Felix)


---

### 3.4. Sprint 3 (03. November - 16. November 2023) <a name="sprint3"></a>



---


### 3.5. Sprint 4 () <a name="sprint4"></a>

---

### 3.6. Sprint 5 () <a name="sprint5"></a>

---