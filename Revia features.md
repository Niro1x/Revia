# Revia Features

## Business Overview

People today rely heavily on their electronic devices — smartphones, laptops, etc. When these devices malfunction, users often face three main problems:

- **Uncertainty** — Not knowing the root cause of the issue or how serious it is.
- **Trust Issues** — Difficulty finding reliable technicians or transparent pricing.
- **Inconvenience** — Managing repairs through multiple communication channels, with poor tracking or unclear timelines.

Revia aims to solve this through a unified digital ecosystem that connects users with verified technicians and uses AI-driven intelligence to streamline diagnosis, matching, and repair management.

---

## Project Overview

Revia is an **AI-powered tech device repair platform** designed to simplify the repair experience for both customers and service providers. It provides:

- **AI-assisted diagnostics** (Revia Assistant) for users who don't know what's wrong with their devices (e.g., phone, laptop, etc.).
- **AI-assisted DIY guide** (Revia Assistant) for repairing the device if possible.
- **Smart provider matching** (Revia Matching Engine) based on specialization, distance, performance, and ratings.
- **Maintenance reminder notifications** (Revia Care) to increase device lifespan.
- **Comprehensive dashboards** for customers, technicians, and administrators to manage Repair Orders and requests.

---

## Core Dashboards

### 1. Customer Platform

This is where users start describing their problem and book technicians to repair their devices.

- Users can describe their problem in simple natural language; **Revia Assistant** interprets the issue and suggests probable causes.
- The request is then forwarded to **Revia Matching Engine** which finds and matches the customer with multiple technicians and suggests them based on multiple criteria.
- Users can track repair orders from beginning of diagnosis for every stage — request, inspection, repair to the receiving of the device fully repaired.
- Automated reminders and notifications to loyal customers (customers with repair history in the platform) for common device issues (e.g., battery health, performance check-ups) based on their device history.

### 2. Provider Platform

- Providers register with credentials, specializations, and business details for admin approval through the admin dashboard.
- **Revia Matching Engine** filters and prioritizes repair requests from customers that best match the provider's skills and proximity.
- Providers can respond to requests, propose custom prices, and set estimated repair times.
- **Earnings & Performance Analytics:** Providers view revenue, ratings, and completion analytics.

### 3. Admin Central Management Dashboard

- Monitors KPIs such as total bookings, revenue, active technicians.
- **User & Provider Management:** Approve certifications, manage disputes, and handle escalations.
- **Category & Transaction Control:** Adjust platform commission, view financial reports, and manage payments or refunds.

---

## Smart Features

> Embedded within the platform's dashboards.

### 1. Revia AI Repair Assistant

A single intelligent repair agent that can hold a conversation with users, understand their device problems, offer accurate repair guidance (DIY solution). If the issue can't be resolved, the assistant generates a report of the conversation and the issues the user described regarding the device and seamlessly escalates them to a certified technician through **Revia Matching Engine**.

- Acts as the **intelligent front door** for users.
- Uses **Natural Language** to interpret user descriptions and guide them through issue identification.
- Suggests probable causes and determines whether it's a self-fixable issue or requires technician assistance.
- Guides users through troubleshooting the device with **DIY solutions**.
- Determines if the user can fix the issue using the DIY solution or should contact a technician.
- If a technician is needed, the assistant generates a **report** containing the details of the device (type, year, brand, approximate issue), forwards it to **Revia Matching Engine** which finds a suitable set of technicians for the specific case itself (customer's request).

### 2. ReviaMatch AI

Connects users with the most suitable, nearby, certified providers. The intelligent matchmaking engine between users and service providers.

- Filters providers within a radius based on specialization, experience, and rating.
- Ranks providers based on availability, success rates, and response time.
- Technicians receive the issue and respond to the customer with price of inspection and time to repair.
- Customer either accepts their response or rejects, which alternatively suggests the user another technician, and the process is repeated couple of times until user finds a suitable candidate.

#### Core System Requirements for Matching

> Should work like DIDI and Uber requests — dynamic matching where the two matching parties suit each other.

- **Input Data:** The system must process both structured data (geographic coordinates, star ratings) and unstructured data (AI-generated diagnostic reports from a conversational assistant).
- **Decision Logic:** The engine uses **Multi-Criteria Decision Making (MCDM)** to balance conflicting factors: technician specialization (skill-fit), physical distance (proximity), historical performance (success rate), and user ratings.
- **Dynamic Routing:** The matching should be "reciprocal," ensuring the technician is capable of the specific repair identified in the diagnostic report (e.g., if the user wants to fix a Samsung phone, recommend a technician that fixes Android and Samsung phones — not iPhones only).

---

## Case Study — The User Journey Scenario

Ahmed's smartphone stops charging. He's unsure whether it's a software or hardware issue.

1. Ahmed visits the platform and selects **"I don't know the problem."**
2. The **AI assistant** chats with him, asking a few guided questions about symptoms.
3. The AI generates a **diagnostic report** suggesting possible causes: *"Battery wear (70%), Charging port issue (30%)."*
4. The system suggests **cleaning the charging port** as the beginning step.
5. Ahmed cleans the port but it still doesn't work.
6. The system sends this request to **technicians within a range** of Ahmed's location.
7. Providers respond with inspection fees, estimated repair costs, and timelines.
8. Ahmed selects one provider, pays the inspection fee, and tracks the repair status via the dashboard.
9. Once the technician finalizes the repair, the system updates the device's **repair history**.
