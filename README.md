# RAG_4_Scratch

![Immagine WhatsApp 2025-12-08 ore 18 20 40_de0ded30](https://github.com/user-attachments/assets/c2ee7fda-e89e-48b5-90d3-066b2c4fb7c5)

# ⚖️ Agentic RAG-Based Legal Question Answering System

## 📌 Project Overview

[cite_start]This project presents a systematic analysis and development of an **Agentic Retrieval-Augmented Generation (RAG)** system designed to handle civil law inquiries across three different jurisdictions[cite: 6]. [cite_start]The system combines localized document retrieval with Large Language Model (LLM) reasoning to generate accurate, source-grounded responses[cite: 7].

[cite_start]We explored two primary architectural approaches: **Single-Agent RAG** and **Multi-Agent RAG**, comparing distinct trade-offs in scalability and retrieval accuracy[cite: 8]. [cite_start]The final system leverages a **Multi-Agent Hybrid** architecture, where a supervisor agent coordinates country-specific sub-agents to ensure high faithfulness and legal relevance[cite: 82, 95].

## 🚀 Key Results

[cite_start]After evaluating various routing strategies (Standard, ReAct, and Hybrid) using **RAGAS**, the **OpenAI Multi-Agent Hybrid** configuration was selected as the final model[cite: 80, 85]. [cite_start]It achieved the best balance between faithfulness to source documents and answer relevance on unseen legal queries[cite: 95].

**Official Evaluation Metrics (Unseen Queries):**

| Metric | Score | Description |
| :--- | :--- | :--- |
| **Context Recall** | **0.867** | [cite_start]High ability to retrieve relevant legal articles/cases [cite: 89] |
| **Context Precision** | **0.800** | [cite_start]High signal-to-noise ratio in retrieved chunks [cite: 89] |
| **Faithfulness** | **0.776** | [cite_start]Strong grounding in the provided legal text [cite: 89] |
| **Answer Relevancy** | **0.546** | [cite_start]Aligning well with the user's intent [cite: 90] |
| **Answer Correctness** | **0.549** | [cite_start]Reliable alignment with reference answers [cite: 90] |

> [cite_start]**Note:** The Multi-Agent approach significantly outperformed Single-Agent modes in grounding, proving that supervisor-based routing enables more effective use of heterogeneous legal sources[cite: 94].

## 🛠 Tech Stack

* **LLM Inference:**
    * [cite_start]**OpenAI:** `GPT-4o-mini` (Selected for final system) [cite: 49]
    * [cite_start]**Hugging Face:** `meta-llama/Llama-3.3-70B-Instruct` (Tested for comparison) [cite: 50]
* **Embeddings:**
    * [cite_start]`text-embedding-ada-002` (OpenAI - 1536 dim) [cite: 23, 24]
    * [cite_start]`sentence-transformers/all-MiniLM-L6-v2` (Local - 384 dim) [cite: 18, 19]
* [cite_start]**Vector Database:** FAISS (Facebook AI Similarity Search) [cite: 29]
* [cite_start]**Evaluation Framework:** RAGAS [cite: 79]
* [cite_start]**Data Format:** Structured JSON (Civil Code Articles & Court Case Decisions) [cite: 10, 13, 14]

## ⚙️ Methodology

### 1. Data Preprocessing & Embedding
[cite_start]The legal corpus resides locally in a structured JSON format, grouped by country and legal domain[cite: 10]. [cite_start]We maintained separate FAISS indexes for each country/domain to support specialized retrieval[cite: 30].
* [cite_start]**Index.faiss:** Stores high-dimensional vector similarities[cite: 32].
* [cite_start]**Index.pkl:** Stores metadata and mappings to original text for context reconstruction[cite: 35].

### 2. System Architectures
We implemented and tested two main architectural flows:

* **Single-Agent RAG:**
    * [cite_start]**Standard Mode:** Direct retrieval and generation (optimized for speed)[cite: 60].
    * [cite_start]**ReAct Mode:** Uses a reasoning-action-observation loop for better interpretability[cite: 63].
    * [cite_start]**Hybrid Legal Mode:** Combines semantic search with metadata filtering (jurisdiction/domain)[cite: 66].

* **Multi-Agent RAG (Supervisor-Worker Pattern):**
    * [cite_start]**Supervisor Agent:** Interprets the query and routes it to the correct jurisdiction[cite: 72].
    * [cite_start]**Sub-Agents:** Country-specific agents that retrieve documents from their dedicated FAISS index and generate partial answers[cite: 76].

### 3. Pipeline Execution
1.  [cite_start]**User Query:** The system receives a legal question[cite: 40].
2.  [cite_start]**Routing/Reasoning:** The Supervisor determines if retrieval is required and selects the relevant domain[cite: 42, 53].
3.  [cite_start]**Vector Search:** Relevant chunks are retrieved via FAISS[cite: 44].
4.  [cite_start]**Generation:** The LLM generates a grounded answer using the retrieved context[cite: 46].
5.  [cite_start]**Output:** Final answer provided with sources and metadata[cite: 47].

