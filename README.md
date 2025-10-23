# Code_O_Holics - MobilityCorp | O'Reilly Architectural Katas Q4 2025: AI-Enabled Architecture

## Team "Code_O_Holics"

- [**Lokkeshwaran J**](https://www.linkedin.com/in/lokkeshwaran/) , Architect
- [**Karthik Balasubramanian**] , Architect
- [**Gayathri Sankara Subramanian**] , Associate Architect
- [**Shanmugam Kothandan**] , Security Solution Architect


# Problem definition

## Context

The objective is to define a new technical architecture for **MobilityCorp**, a short-term rental provider of last-mile transport (electric scooters, eBikes, cars, and vans) operating in city and suburban locations.

The new architecture must strategically incorporate **AI tooling** to enhance the solution for both the company and its customers.

## Business Challenges 

The architecture must address three core business challenges:

### 1. Vehicle Distribution Imbalance

* **Problem:** Customers complain that the right vehicles aren't in the right places.
* **Need:** The company needs to **anticipate customer needs** and determine when people will want to use the vehicles.

### 2. Battery Management

* **Problem:** Electric vehicles are running out of charge.
* **Need:** For bikes and scooters, the company needs a system to prioritize and plan staff visits for **battery pack swaps and vehicle redistribution**.

### 3. Customer Engagement

* **Problem:** Most customers use the service ad-hoc.
* **Goal:** The goal is to encourage customers to **rely on the fleet for regular trips** like daily commutes.

## Key Objective

The Key Objective is to build a Unified, AI-Driven Mobility Platform that achieves maximum operational efficiency and fleet utilization while providing a seamless, reliable, and personalized customer experience across a mixed fleet of vehicles (Cars, Vans, Bikes, and Scooters).

**In short:** Maximize Fleet Utilization and Customer Experience through AI-Powered Automation.

# MobilityCorp Platform - Big Picture Understanding

The below diagram is arranged to show the full operational cycle, with distinct pathways for real-time vehicle data, customer transactions, and AI-driven operations of MobilityCorp Platform.

![](/assets/Mobility_Corp_BigPicture.jpg)

1\. Edge & Data Ingestion (Telemetry Lifecycle)
--------------------------------------------

1.  **Vehicle TCU Boxes** (in Cars, Vans, Bikes, and Scooters) capture **real-time GPS, lock status, and fault data** and push it to the cloud.

2.  Data flows through a high-speed **MQTT Broker** and **Ingestion Service** to ensure reliable, low-latency processing.

3.  The data is **Enriched** and persisted in the **Telemetry/Status DB (NoSQL)**, creating a source of truth for all vehicle activity.

* * * * *

2\. Core Services & Customer Interaction (Transaction Lifecycle)
----------------------------------------------------------------

1.  The **Customer App** initiates **Bookings, Payments (per minute), and Fines** (for wrong location/late return).

2.  **Core Services** manage all business logic, including **Payment Processing**, **Fines**, and **Immobilization**. This service sends control commands (Lock/Unlock) to the **TCU** and records transactions in the **Booking/Payment/Profile DB (SQL)**.

3.  **Vehicle Return (Car/Van)** is confirmed via the **Customer App** submitting **photo proof** and **plug-in notification** (for EVs) to the Core Services.

* * * * *

3\. AI & Optimization (Operational Efficiency)
----------------------------------------------

1.  The **AI Layer** consumes large datasets from the databases:

    -   **Dynamic Demand Predictor (ML)** forecasts where and when vehicles will be needed.

    -   **Fleet & Charge Optimizer (RL)** uses demand forecasts and real-time battery status to calculate the **optimal task list** for staff (where to swap batteries, where to move vehicles).

    -   **Generative Incentivizer (LLM)** creates personalized customer notifications.

2.  **Core Services** dispatch these optimized tasks (Swaps, Redistribution, etc.) to the **Operations Staff App**.

3.  Staff completes tasks and sends back **Task Completion (Feedback)** data, closing the operational loop.

# Solution


### How AI Is Used to Solve the Problems

| **Business Problem** | **AI Component** | **Description of Use** |
| --- | --- | --- |
| **Vehicle Distribution Imbalance** | **Dynamic Demand Predictor** (Supervised ML) | Uses historical GPS data, weather, events, and public transit schedules to forecast vehicle demand (down to the hour and parking spot). This generates a **Targeted View** called a *Redistribution Task Queue* for operations staff. |
| **Battery Management** | **Charging/Swap Optimizer** (Reinforcement Learning / Optimization) | Prioritizes battery swap/charging tasks for staff. It uses real-time battery levels, predicted demand from the **Dynamic Demand Predictor**, and staff current location/vehicle type (van vs. car) to determine the optimal route and sequence for maximum fleet uptime. |
| **Customer Engagement** | **Generative Commute Incentivizer** (Generative AI/LLM) | Uses user trip history and predicted regular commute patterns to generate **innovative, personalized, and timely** promotional messages and incentives (e.g., "Use a scooter for your morning commute three times this week and get 50% off the third trip"). |


## Architecture Designs


### 1\. Dynamic Demand Predictor - Operations Dashboard View

#### Solution Approach
- Provide staff with an actionable list to move scooters and eBikes to where demand is predicted to be highest.

- **Redistribution Task Queue.** - A table showing: `Source Parking Spot`, `Destination Parking Spot`, `Vehicle Type (eBike/Scooter)`, `Number of Vehicles to Move`, and `Priority (High/Medium/Low)`.

- The Dynamic Demand Predictor model processes GPS and usage data to predict future supply-demand imbalances, generating the move commands as its output.

##### Data Flow :
![](/assets/DynamicDemand.png)

#### Adding GenAI Flavor to the Solution

- **Generative Scenario Planning Engine:** An LLM or specialized generative model is trained on historical data, operational policies, and a library of "successful redistribution strategies." It receives the demand forecast (from the existing ML model) and generates **human-readable, policy-compliant, and prioritized redistribution action scripts** for field managers, complete with justifications.
- The output is not just a list of commands, but a structured text/JSON "Action Script" that includes justification, risk assessment, and recommended driver incentives for that specific action.

##### Data Flow :
![](/assets/DynamicDemandGenAI.png)


### 2\. Charging/Swap Optimizer - Staff Mobile App View

#### Solution Approach

- Provide field staff with the most efficient, prioritized route for maintenance tasks.

-   **Optimized Swap/Charge Route.** -  A real-time map displaying the staff member's current location, the sequence of parking spots to visit, and the reason for the visit (e.g., "Swap 2 batteries," "Plug in Van 101").

-   The Optimizer processes battery levels and demand forecasts to output a route sequence that minimizes fleet downtime and maximizes utilization.

##### Data Flow :
![](/assets/IntelligentSwapping.png)

#### Adding GenAI Flavor to the Solution
- The biggest challenge for an RL optimizer is training on rare, high-consequence scenarios (e.g., a major city blackout or a sudden, unexpected street closure). Generative AI can solve this data scarcity problem.
- **Synthetic Edge-Case Generator (VAE/GAN):** A generative model is trained on real-world vehicle failure, battery degradation, and disaster data. It generates millions of realistic, but non-existent, "synthetic crisis scenarios" (e.g., 50 low-battery bikes simultaneously impacted by a 1-hour traffic gridlock).

##### Data Flow :
![](/assets/IntelligentSwappingGenAI.png)


### 3\. Generative Commute Incentivizer - Customer Mobile Notification

#### Solution Approach

- Increase customer loyalty and shift from ad-hoc to regular-commuter usage.
- **Personalized Incentive Notification** - A personalized, natural-language push notification to the customer. For example: *"Hey LJ! ☀️ We see you love the eBike. Take $5 off your next 5 morning rides this month and make it your daily commute!"*
-  The LLM receives a prompt with a user's usage profile and a business goal (e.g., "increase weekday morning rides") and outputs the natural-language text for the incentive.

##### Data Flow :
![](/assets/CustomerEngagement.png)


# Architectural Decision Records (ADRs)

- ([**ADR-001_Implement an AI Service Abstraction Layer**](/ADRs/ADR-001_ImplementanAIServiceAbstractionLayer.md))
- ([**ADR-002_Adopt a Shadow-Mode and Human-in-the-Loop Validation Strategy for AI Output**](/ADRs/ADR-002_AdoptaShadow-ModeandHuman-in-the-LoopValidationStrategyforAIOutput.md))
- ([**ADR-003_Utilize a Reinforcement Learning Model for Charging Optimization**](/ADRs/ADR-003_UtilizeaReinforcementLearningModelforChargingOptimization.md))

# Conclusion

This solution suite establishes the MobilityCorp platform as a **proactive**, **intelligent**, and highly **automated** ecosystem built for navigating the complexities of multi-modal fleet operations. By strategically deploying Generative AI and advanced machine learning across all critical business functions, the platform achieves the core objective of maximizing fleet utilization and customer lifetime value.