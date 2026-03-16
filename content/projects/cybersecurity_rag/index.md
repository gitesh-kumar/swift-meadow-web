---
title: "Enhancing Cybersecurity Intelligence: A RAG Framework for Fine-Tuned LLMs"
date: 2025-06-02
summary: "Master's Thesis: Developing a synergetic RAG and Fine-Tuning framework to eliminate hallucinations in CVE vulnerability analysis."
tags: ["LLMs"]
tech_stack: ["Llama-2", "PyTorch", "FAISS", "PEFT/LoRA"]
links:
  - type: github
    url: https://github.com/gitesh-kumar/Llama2-RAG
    label: Code
featured: true
---

## Thesis Core Objective
The central goal of this research was to solve the **static knowledge limitation** and **factual inaccuracy** of Large Language Models (LLMs) in the rapidly shifting cybersecurity landscape. This project proves that **Fine-Tuning** and **Retrieval-Augmented Generation (RAG)** are not redundant but synergetic: Fine-Tuning adapts the model to the technical "language" of security, while RAG provides the factual "evidence".

---

## Methodology & Technical Points

### 1. Domain Adaptation (Fine-Tuning)
I utilized **Parameter-Efficient Fine-Tuning (PEFT)** with **LoRA** to adapt Meta-Llama-3 to the cybersecurity domain. 
* **Linguistic Specialization:** The model was trained to understand and generate structured security reports rather than general text.
* **Chatment Data Augmentation:** Implemented a custom "Chatment" strategy to expand the training set to 14,000 instructions, ensuring robustness across diverse user queries.

### 2. Dynamic Contextualization (RAG)
To prevent hallucinations regarding specific vulnerability details, I built a real-time retrieval pipeline.
* **FAISS Knowledge Base:** Ingested 25 years of MITRE CVE data (1999–2024) into a high-performance vector store.
* **Grounding:** The system retrieves the exact technical documentation for a CVE before the LLM generates a response, ensuring the output is based on facts, not just training memory.

### 3. Progressive Training Strategy
* **Phased Learning:** The model underwent a multi-stage process to transition from general reasoning to specialized vulnerability analysis.
* **Stability:** Used a low learning rate (2e-5) to ensure the model retained its foundational intelligence while layering on new security expertise.

---
---
## 📊 Quantitative Evaluation Results

The performance of the framework was rigorously measured against three baseline configurations: the **Base Llama-2**, an **Instruction-Fine-Tuned** version, and a **Progressive Fine-Tuned (No RAG)** version. The results below highlight how the combination of domain-specific training and real-time retrieval optimizes different aspects of security analysis.

### 1. Semantic Accuracy (METEOR)
METEOR was our primary metric for evaluating the technical "expert-level" quality of the responses. It rewards the model for using correct cybersecurity synonyms and technical context.
![METEOR Score Comparison](/uploads/Meteor.png)
> **Key Finding:** The RAG-enhanced model showed the highest METEOR scores, proving that retrieved context helps the model speak like a professional security analyst.

### 2. Factual Precision (BLEU)
We used BLEU to verify the model's ability to extract exact identifiers, such as CVE IDs, dates, and CVSS scores, without hallucinating.
![BLEU Score Comparison](/uploads/BLEU.png)
> **Key Finding:** By grounding the model in retrieved MITRE data, we effectively eliminated the "ID Hallucination" problem common in standard LLMs.

### 3. Meaning & Reasoning (BERTScore)
BERTScore uses contextual embeddings to see if the model's answer means the same thing as the ground truth, even if the phrasing is different.
![BERTScore Comparison](/uploads/Bertf1.png)
> **Key Finding:** High F1-scores across the board for the RAG model indicate that the underlying security reasoning remains robust and accurate.

### 4. Structural Alignment (ROUGE-L)
ROUGE-L measures the longest common subsequence to see if the model follows the standard formatting expected in vulnerability disclosures.
![ROUGE-L Comparison](/uploads/ROUGESCORES.png)
> **Key Finding:** Progressive fine-tuning taught the model the *structure* of a report, while RAG provided the *content* to fill it correctly.

---

### 🏆 Evaluation Summary
The data confirms that while **Fine-Tuning** improves the model's professional tone and structural adherence, **RAG** is the essential component for ensuring factual reliability. Together, they achieve a state-of-the-art balance for automated cybersecurity intelligence.
## 📊 Results & Key Findings
The framework was evaluated using **METEOR**, **ROUGE-L**, and **BLEU** metrics to compare performance against a base model.

* **Hallucination Mitigation:** The RAG architecture significantly reduced errors in specific identifiers like software versions and patch numbers.
* **Resilience:** Even when the retriever fetched generic information, the fine-tuned model's domain knowledge maintained a higher accuracy baseline than the standard model.
* **Accuracy:** The multi-stage fine-tuning combined with RAG boosted the model’s ability to generate accurate, structured cybersecurity outputs.

---
**Author:** Gitesh Kumar  
**Institution:** Friedrich-Alexander-Universität Erlangen-Nürnberg (FAU)  
**Institute:** Factory Automation and Production Systems (FAPS)