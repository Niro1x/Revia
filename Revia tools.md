# Revia Core Engines: Technical Architecture

## Deep Dive: AI Assistant & Intelligent Matching Engine

This document focuses exclusively on the two primary intelligent systems of the Revia platform. These engines handle the core value proposition: translating an unstructured user problem into a structured data format, and mathematically matching that problem to the optimal service provider.

---

## Engine 1: Revia Assistant (The AI Diagnostician)

**Goal:** Act as the intelligent "front door." It must understand natural language, attempt a DIY resolution, and generate a machine-readable report if a technician is required.

### 1. Assistant Technology Stack

- **LLM Provider:** Google AI Studio (Gemini 1.5 Flash)
- **Integration:** Python (via `google-generativeai` SDK or `aisuite`)
- **Evaluation:** TruLens (for measuring diagnostic accuracy and hallucination rates)

### 2. Assistant Operational Pipeline

The Assistant operates in a strict three-step loop to prevent it from acting like a generic chatbot.

#### Step 1: Symptom Extraction (Natural Language)

- **Mechanism:** The system prompt forces Gemini to ask one question at a time.
- **Example:** User says "My phone is broken." The AI asks clarifying questions to narrow down whether it's software, screen, battery, or charging port.

#### Step 2: DIY Protocol

- **Mechanism:** Once the issue is identified, the AI consults its knowledge base to suggest a safe DIY fix (e.g., "Use a wooden toothpick to clean the charging port").
- **Branching:** The AI asks: "Did this resolve the issue?" If yes, the session ends. If no, it triggers Step 3.

#### Step 3: Structured Escalation (JSON Output)

- **Mechanism:** The AI stops conversing and summarizes the entire chat history into a strict, predefined JSON schema.
- **Output Example:**

```json
{
  "device_type": "Smartphone",
  "brand": "Samsung",
  "model": "S21",
  "symptom": "Does not charge",
  "diy_attempted": ["Cleaned port"],
  "probable_cause": "Hardware/Charging Port",
  "urgency_level": "High"
}
```

> **Critical Handoff:** This JSON object is the exact payload sent to the Matching Engine.

---

## Engine 2: Revia Matching Engine (The Core Logic)

**Goal:** Ingest the structured JSON report and use Multi-Criteria Decision Making (MCDM) to rank and route the job to the best technicians.

### 1. Matching Engine Technology Stack

- **Semantic Engine:** Python (SentenceTransformers or Gemini Text Embeddings)
- **Decision Logic:** Python (Custom MCDM mathematical algorithms)
- **State Management:** Firebase Cloud Functions (for handling the real-time "bidding" timeout)

### 2. Matching Engine Operational Pipeline

The engine processes the JSON report through three distinct layers.

#### Step 1: Semantic Search / RAG (The Filter)

- **Mechanism:** A keyword search for "does not charge" might miss a technician whose profile says "Power distribution specialist." We use **vector embeddings** to turn the JSON's `probable_cause` and the Technician's skills list into numbers.
- **Action:** The engine calculates the **Cosine Similarity** between the user's problem and the provider's skills, filtering out anyone who isn't a technical match.

#### Step 2: MCDM Algorithm (The Ranker)

- **Mechanism:** The engine now has a pool of capable technicians. It must rank them based on conflicting criteria.

- **The Formula:**

  `Score = (W₁ × SkillMatch) + (W₂ × 1/Distance) + (W₃ × Rating) + (W₄ × SuccessRate)`

- **Customization:** You define the Weights (W). For example, if "Urgency" in the JSON is "High," the engine dynamically increases W₂ (Distance) so closer technicians rank higher.

#### Step 3: Reciprocal Dynamic Routing (The Handshake)

- **Mechanism:** The system does not just assign the job; it facilitates a marketplace.
- **Action:**
  1. The Repair Order is broadcast to the **Top 3** ranked technicians.
  2. Technicians review the AI-generated JSON report.
  3. Technicians respond with an **Inspection Fee** and **Estimated Time**.
  4. The User reviews these 3 bids and clicks **"Accept"** on their preferred option.

---

## The Integration: How They Work Together

The true innovation of the Revia platform is the handoff between these two engines.

- **Unstructured to Structured:** The user types messy, confusing human language. The Assistant cleans it, diagnoses it, and packages it into clean Data (JSON).
- **Data to Action:** The Matching Engine consumes this clean Data, runs high-level mathematics (MCDM + Vectors) to find the perfect human-to-human match, and initiates the real-world repair process.

---

## 3. Tech Stack Categorization Summary

Below is the definitive mapping of every tool explored during the research phase, split into four distinct categories and ranked by implementation priority.

### 🟢 Category 1: Assistant Only

Tools used specifically to build, connect, and evaluate the AI Diagnostician.

**High Priority Tools:**

- **aisuite (Andrew Ng's Repo):** The Python connector library. Used to cleanly integrate the LLM into your code, allowing you to quickly call models and extract the JSON payload without bloating your codebase.
- **TruLens:** The evaluation grader. Crucial for academic grading; used to measure the Assistant's responses for "hallucinations" and relevance to ensure safe and accurate DIY advice.

### 🔵 Category 2: Matching Engine Only

Tools used specifically for the backend database, ranking mathematics, and routing logic.

**High Priority Tools:**

- **Firebase (Firestore & Auth):** The real-time database. Essential for storing technician profiles, handling the "Uber-style" bidding timeouts, and managing the live, dynamic state of reciprocal repair orders.
- **Langflow:** (Backup/Prototyping Tool) A visual node-based builder. Used to map out the complex RAG retrieval and MCDM logic flow if writing it directly in raw Python becomes too difficult to visualize.

### 🟣 Category 3: Both (Core Infrastructure)

Tools that form the backbone of the entire Revia platform.

**High Priority Tools:**

- **Google AI Studio:** The primary AI powerhouse. It provides Gemini 1.5 Flash for the Assistant's diagnostic chat, and Gemini Text Embeddings for the Matching Engine's semantic vector search.
- **Streamlit:** The frontend web application framework. It hosts the chat UI for the Assistant and the multiple dynamic dashboards (Customer, Tech, Admin) for the Matching Engine.
- **Hugging Face (and Hugging Face Cookbook):** The open-source resource hub. Used to find hardware/e-waste conversational datasets for the Assistant's system prompt, and to source SentenceTransformers models for the Matching Engine's vector search.

**Secondary/Reference Tools:**

- **mlabonne/llm-course (GitHub):** The technical "Textbook." Used to study Prompt Engineering techniques (for the Assistant) and learn how to implement RAG architecture from scratch (for the Matching Engine).
