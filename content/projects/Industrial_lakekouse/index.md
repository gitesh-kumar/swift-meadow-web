---
title: "Industrial Agentic AI System (Predictive Maintenance & Financial Risk Engine)"
date: 2026-03-11
summary: "A full-stack industrial data pipeline with dual-mode AI agents, zero-trust security and a live deployed dashboard which translates raw IoT telemetry into real-time financial risk insights."
tags: ["Data-Eng", "AI Agents", "IoT", "Zero-Trust", "LLMOps"]
tech_stack: ["Python 3.12", "LangChain", "Groq", "Ollama/Llama-3", "OpenZiti", "FastAPI", "Docker", "SQLAlchemy", "Railway"]
links:
  - type: github
    url: https://github.com/gitesh-kumar/Industrial_Data_Pipeline
    label: Code
  - type: live
    url: https://industrialdatapipeline.up.railway.app/dashboard
    label: Live Demo
featured: true
---

## Project Overview

Most industrial monitoring systems tell you when something is wrong. This one tells you what it's costing you right now and lets you ask questions about it in plain English.

I built this to solve a problem I saw firsthand at Hindustan Petroleum which was critical operational data locked in systems engineers couldn't easily query, leading to decisions made from stale Excel exports pulled manually from SCADA systems. The goal was to automate that entire workflow from raw sensor ingestion to financial decision support and make it queryable by a non-technical plant manager in natural language.

---

## Data Architecture (Medallion Pattern)

Raw sensor telemetry from 17 factory assets (turbines, compressors, pumps, motors, generators, heat exchangers) flows through three layers:

**Bronze** - High-frequency ingestion, append-only, never overwritten. Each machine generates readings every 3 seconds: vibration RMS, temperature, and power consumption. Failure spikes are injected probabilistically to simulate real anomaly events.

**Silver** - Removing duplicates, quality flagging (PASS, HIGH_TEMP_WARN, VIBRATION_CRITICAL, FAILURE_SPIKE, DEGRADATION_WARN), unit conversion and composite risk scoring per machine.

**Gold** - Live German energy market pricing applied to each machine's power consumption to calculate hourly energy cost in euros. Machines are classified by efficiency status and given operational recommendations: CONTINUE, REDUCE_LOAD, SCHEDULE_MAINTENANCE, or IMMEDIATE_SHUTDOWN.

---

## AI Agent (Three Deployment Modes)

The system supports three inference modes, each designed for a different deployment context.

**Local Mode (Ollama + Llama3)** - Runs entirely on-premise. No data leaves the machine. Built for environments where operational data cannot be sent to external cloud providers which is the reality in most industrial settings.

**Cloud Fast Mode (Groq + Intent Router)** - The AI classifies the question into one of eight predefined intents, executes a pre-validated parameterized SQL query and uses the LLM only to phrase the answer naturally. Two LLM calls total. This replaced the original ReAct agent after I found it looping on certain questions and burning through token budgets unpredictably. Token usage dropped by roughly 80%.

**Cloud Agent Mode (Groq + ReAct)** - A LangChain SQL agent that autonomously writes and executes SQL for exploratory analysis. More flexible but deliberately kept separate from the production fast mode as an exploratory tool.

The dual-mode design reflects a real industrial constraint, which is, data sovereignty versus scalability. The same codebase handles both just switched by a single environment variable.

---

## Hallucination Bug and Grounding Guards

During testing I found a critical failure. The model confidently answered questions about machine types that didn't exist in the data, fabricating a plausible-sounding answer using unrelated fleet statistics. I implemented four grounding guards:

1. **Unrecognized intent refusal** - Returns a clear message instead of defaulting silently to a fallback query.
2. **Entity verification** - Checks whether the question references a machine type that actually exists in the schema before querying.
3. **Empty result handling** - Explicit response when no data matches rather than letting the model improvise.
4. **Grounded prompt** - Instructs the model to reference only machine IDs present in the retrieved data.

---

## Zero-Trust Security Layer (OpenZiti)

Each of the 17 simulated machines holds a real X.509 cryptographic identity issued by a self-hosted Ziti controller running on a Hetzner Linux server in Germany. Data travels through an encrypted OpenZiti overlay network with no exposed ports. The ingestion service is completely dark to the internet and accessible only through the Ziti fabric.

Every connection is logged in a cryptographically-verifiable audit trail. If a machine identity is revoked, access terminates immediately without touching any other machine or reconfiguring the network.

---

## Data Governance

An automated governance audit runs after every pipeline execution:

- **Pipeline Health Score** - Percentage of records passing the Silver quality gate.
- **Data Freshness Check** - Flags stale ingestion before Gold layer computation.
- **Safety Threshold** - If health drops below 90%, downstream analytics are flagged as unreliable.

---

## Deployment

- FastAPI REST service with 5 endpoints (`/machines`, `/machines/{id}/risk`, `/pipeline/health`, `/query`, `/dashboard`)
- Industrial command center dashboard has machine status cards with risk scores, AI query interface and pipeline health view.
- Containerised with Docker.
- Deployed on Railway with GitHub CI/CD.
- Zero-trust ingestion layer on self-hosted Hetzner infrastructure (Nuremberg, Germany).

---

## Technical Metrics

| Metric | Detail |
| :--- | :--- |
| **Machines monitored** | 17 assets across 6 machine types |
| **Pipeline layers** | Bronze → Silver → Gold medallion |
| **AI deployment modes** | 3 (local, cloud fast, cloud agent) |
| **Token reduction** | ~80% vs original ReAct agent |
| **Security** | Zero-trust, no exposed ports, X.509 per-machine identity |
| **Deployment** | Live on Railway, Docker, GitHub CI/CD |

---

**Author:** Gitesh Kumar  
**Project Context:** Industrial IoT, Agentic AI, LLMOps, OT/IT Security  
**Tech:** Python, FastAPI, LangChain, Groq, Ollama, OpenZiti, Docker, SQLAlchemy, Railway