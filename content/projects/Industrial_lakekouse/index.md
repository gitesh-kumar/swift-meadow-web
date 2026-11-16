---
title: "Industrial Agentic AI System (Predictive Maintenance & Financial Risk Engine)"
date: 2026-03-11
summary: "A full-stack industrial data pipeline with a SQL-writing LangChain agent, an MCP server, OPC UA ingestion, zero-trust security and a live deployed dashboard which translates raw IoT telemetry into real-time financial risk insights."
tags: ["Data-Eng", "AI Agents", "MCP", "OPC UA", "IoT", "Zero-Trust", "LLMOps"]
tech_stack: ["Python 3.12", "LangChain", "MCP", "OPC UA (asyncua)", "Groq", "Ollama", "Langfuse", "LangSmith", "OpenZiti", "FastAPI", "Docker", "SQLAlchemy", "Railway"]
links:
  - type: github
    url: https://github.com/gitesh-kumar/Industrial_Data_Pipeline
    label: Code
  - type: live
    url: https://industrialdatapipeline.up.railway.app/dashboard
    label: Live Demo
  - type: live
    url: https://industrialdatapipeline-mcp.up.railway.app/
    label: MCP Server Demo
featured: true
---

## Project Overview

Most industrial monitoring systems tell you when something is wrong. This one tells you what it's costing you right now and lets you ask questions about it in plain English.

I built this to solve a problem I saw firsthand at Hindustan Petroleum which was critical operational data locked in systems engineers couldn't easily query, leading to decisions made from stale Excel exports pulled manually from SCADA systems. The goal was to automate that entire workflow from raw sensor ingestion to financial decision support and make it queryable by a non-technical plant manager in natural language.

All machine data is simulated.

---

## Data Architecture (Medallion Pattern)

Raw sensor telemetry from 17 factory assets (turbines, compressors, pumps, motors, generators, heat exchangers) flows through three layers:

**Bronze** - High-frequency ingestion, append-only, never overwritten. Each machine generates readings every 3 seconds: vibration RMS, temperature, and power consumption. Failure spikes are injected probabilistically to simulate real anomaly events. The machines publish their values through an OPC UA server (85 nodes, five variables per machine), and an OPC UA client subscribes to them and writes the bronze layer. The pipeline reads the machines the way it would read a real plant.

**Silver** - Removing duplicates, quality flagging (PASS, HIGH_TEMP_WARN, VIBRATION_CRITICAL, FAILURE_SPIKE, DEGRADATION_WARN), unit conversion and composite risk scoring per machine.

**Gold** - Live German energy market pricing applied to each machine's power consumption to calculate hourly energy cost in euros. Machines are classified by efficiency status and given operational recommendations: CONTINUE, REDUCE_LOAD, SCHEDULE_MAINTENANCE, or IMMEDIATE_SHUTDOWN.

---

## AI Agent

One LangChain agent with one tool, `run_sql`. The model reads a description of the database, writes the SQL itself, runs it through the tool and answers from the rows it gets back. If a query fails, it reads the error and tries again, with a hard limit of five queries per question.

**Why an agent and not a router.** The first production version was an intent router. It sorted each question into one of eight fixed intents and ran a prewritten query. It was cheap and predictable, but it gave no answer whenever the wording did not match an intent. The agent removes that limit. The model writes the SQL, so the wording no longer matters.

**Reasoning beyond the data.** Many real questions have no column that answers them. When a plant manager asks which machine is the most inefficient, nothing in the data is called efficiency. The agent says what is not measured, picks the connected measurements, queries them, compares the machines, and labels its answer as an inference. A question that has nothing to do with the factory gets a plain answer without any query.

**Two deployment modes**, switched by one environment variable:

- **Cloud** - Groq. If the model provider fails after a query has already run, the system returns the raw rows with a notice instead of an error.
- **Local** - Ollama on the same machine. No data and no traces leave the machine. Built for environments where operational data cannot be sent to external cloud providers, which is the reality in most industrial settings. Answer quality depends on the model size.

---

## Safety: Read-Only SQL Guard and Grounding

The model is never trusted to be safe. A guard written as plain code sits between the model and the database and checks every query in four layers: the database is opened read-only, a SQLite authorizer allows reads on listed tables only, only a single SELECT statement is accepted, and a timeout and a row limit apply. A prompt that tries to make the model delete data or show other tables fails at the guard, whatever the model does.

**The hallucination bug.** During testing of the first version I found a critical failure. The model confidently answered questions about machine types that didn't exist in the data, fabricating a plausible-sounding answer using unrelated fleet statistics. That bug shaped the design. In the current version every number and machine id in an answer must come from a query result, the prompt demands it, and the evaluation checks it for every test question.

---

## Observability and Evaluation

**Tracing.** In cloud mode every run is traced to Langfuse and LangSmith: the prompt, each tool call, the SQL, the rows and the answer. Tracing is switched off in local mode, so the on-premise promise holds.

**Evaluation.** A test set of 39 questions covers direct answers, inference questions, refusals, safety attacks and questions the data cannot answer. Each answer is graded by rules, including whether every number in it appears in the query results. The grader itself is tested with a scripted good model and a scripted bad model, so I know it catches invented numbers before it judges a real model. It cannot catch shallow reasoning when the numbers are correct, and I treat that as a known limit.

---

## MCP Server

The same data is available as a Model Context Protocol server, so any MCP client (Claude Desktop, Cursor, another agent) can use it without a change to the project. It has three tools: `describe_database`, `run_sql` and `ask_factory`. All of them are read-only and go through the same SQL guard. It runs over stdio locally and over Streamable HTTP on Railway. The public page lets a visitor send real MCP requests from the browser, including a DELETE that the guard rejects. The model-backed tool is off on the public server, and where it is on it has a length limit, a cache and a rate limit.

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
- A second Railway service runs the MCP server.
- Containerised with Docker.
- Deployed on Railway with GitHub CI/CD.
- Zero-trust ingestion layer on self-hosted Hetzner infrastructure (Nuremberg, Germany).

---

## Technical Metrics

| Metric | Detail |
| :--- | :--- |
| **Machines monitored** | 17 assets across 6 machine types |
| **Pipeline layers** | Bronze → Silver → Gold medallion |
| **Ingestion** | OPC UA server and client, 85 nodes |
| **AI deployment modes** | 2 (cloud, local) |
| **MCP tools** | 3, all read-only |
| **Evaluation** | 39 graded test questions |
| **Observability** | Langfuse and LangSmith tracing |
| **Security** | Read-only SQL guard, zero-trust network, no exposed ports, X.509 per-machine identity |
| **Deployment** | Live on Railway, Docker, GitHub CI/CD |

---

**Author:** Gitesh Kumar  
**Project Context:** Industrial IoT, Agentic AI, LLMOps, OT/IT Security  
**Tech:** Python, FastAPI, LangChain, MCP, OPC UA, Groq, Ollama, Langfuse, OpenZiti, Docker, SQLAlchemy, Railway