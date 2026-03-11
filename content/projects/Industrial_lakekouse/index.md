---
title: "Industrial IoT: Predictive Maintenance & Financial Lakehouse"
date: 2026-03-11
summary: "A full-stack Medallion architecture pipeline with a ReAct AI Agent, translating raw IoT telemetry into real-time financial risk insights."
tags: ["Data-Eng", "AI Agents", "IoT"]
tech_stack: ["Python 3.12", "LangChain", "Ollama/Llama-3", "SQLAlchemy", "Power BI"]
links:
  - type: github
    url: https://github.com/gitesh-kumar/Industrial_Data_Pipeline
    label: Code
featured: true
---

## Project Core Objective
The primary goal of this project was to move beyond simple "threshold alerts" in manufacturing. I developed a **Full-Stack Data Engineering Pipeline** that translates raw mechanical telemetry (vibration and temperature) into **Real-Time Financial Risk**. By correlating sensor anomalies with live energy market costs via external APIs, the system enables Profit-First operational decisions.

---

## Methodology & Technical Points

### 1. Medallion Data Architecture
The pipeline follows a modular **Medallion Architecture** to ensure rigorous data quality and lineage:
* **Bronze Layer (Ingestion):** Multi-threaded simulation of high-frequency IoT sensors capturing vibration, temperature, and power metrics.
* **Silver Layer (Transformation):** Implements a **Quality Gate** that deduplicates records and applies industrial standards to flag Sensor Drift or critical vibration thresholds.
* **Gold Layer (Analytics):** High-level business logic integrating an **External Energy Market API** to calculate the `hourly_energy_cost_eur` for every active machine.



### 2. AI Agent: "The Plant Manager"
Instead of a static dashboard, this project features an **Autonomous SQL Agent** acting as a virtual Plant Manager.
* **Framework:** Built using **LangChain** and **Ollama**.
* **Reasoning Engine:** Powered by **Local Llama-3** for 100% offline inference, ensuring sensitive industrial telemetry remains secure.
* **Logic (ReAct Strategy):** The agent utilizes a **Reason/Act** loop. It analyzes a user's natural language question, generates a precise SQL query, executes it against the **Gold Layer**, and interprets the financial impact.



#### Sample AI Reasoning Trace
> **User:** *"Which machine should I prioritize for repair based on financial loss?"*
>
> **AI Agent:** "Based on current telemetry and energy prices of **€198/MWh**, you should prioritize **COMPRESSOR_03**. It currently accounts for **63.14%** of total factory hourly loss due to thermal inefficiency."

---

## Engineering Features

###  Data Governance & Pipeline Health
To ensure the reliability of the executive dashboard, I implemented an automated **Governance Audit** (`data_governance.py`):
* **Health Scorecard:** The system calculates a **Pipeline Health %** based on data freshness and the ratio of records passing the Silver Quality Gate.
* **Safety Thresholds:** If the health score drops below **90%**, the orchestrator issues a critical warning to prevent decisions based on corrupted data.

### Business-Driven Logic
* **Energy Correlation:** The system identifies Thermal Waste and multiplies that inefficiency by the current market rate.
* **Automated Recommendations:** The pipeline generates a `SHUTDOWN` recommendation if a machine is both inefficient and operating during peak-cost energy hours.

---

## Technical Performance Metrics

| Metric | Achievement |
| :--- | :--- |
| **Pipeline Reliability** | Quality-check flags 100% of anomalous readings. |
| **Data Integrity** | Governance audit ensures 90%+ quality before Gold layer. |
| **Inference Security** | 100% Local (No external data leaks via Ollama). |

---

**Author:** Gitesh Kumar  
**Project Context:** Industrial IoT & Agentic Data Engineering  
**Tech:** Python, SQL, LangChain, Llama-3, Power BI