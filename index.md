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
2. [Technical documentation](#technical) 
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

### 1.5. Web (Daniel Rotter) <a name="daniel"></a>

### 1.6. Security (Armin Simma) <a name="armin"></a>



## 2. Technical documentation <a name="technical"></a>


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

### 3.3. Sprint 2 (13. Oktober - 27. Oktober 2023) <a name="sprint2"></a>
*Author*: Bianca

**Team**: Define database scheme

**Individual responsibilities**:

- *Embedded* (implemented by Lucas): 
Define Mbus protocol, test hardware

- *Database* (implemented by Dominik):
Setup necessary tables

- *Organizational stuff* (implemented by Bianca and Felix):
Documentation Github pages, Azure management, time schedule


---

### 3.4. Sprint 3 (30. Oktober - 13. November 2023) <a name="sprint3"></a>

---


### 3.5. Sprint 4 () <a name="sprint4"></a>

---

### 3.6. Sprint 5 () <a name="sprint5"></a>

---