![Immagine WhatsApp 2025-12-08 ore 18 20 40_de0ded30](https://github.com/user-attachments/assets/c2ee7fda-e89e-48b5-90d3-066b2c4fb7c5)

# ⚖️ Agentic RAG-Based Legal Question Answering System

## 📌 Project Overview

This project presents a systematic analysis and development of an **Agentic Retrieval-Augmented Generation (RAG)** system designed to handle civil law inquiries across three different jurisdictions. The system combines localized document retrieval with Large Language Model (LLM) reasoning to generate accurate, source-grounded responses.

We explored two primary architectural approaches: **Single-Agent RAG** and **Multi-Agent RAG**, comparing distinct trade-offs in scalability and retrieval accuracy. The final system leverages a **Multi-Agent Hybrid** architecture, where a supervisor agent coordinates country-specific sub-agents to ensure high faithfulness and legal relevance.

## 🚀 Key Results

After evaluating various routing strategies (Standard, ReAct, and Hybrid) using **RAGAS**, the **OpenAI Multi-Agent Hybrid** configuration was selected as the final system. It achieved the best balance between faithfulness to source documents and answer relevance on unseen legal queries.

**Official Evaluation Metrics (Unseen Queries):**

| Metric | Score | Description |
| :--- | :--- | :--- |
| **Context Recall** | **0.867** | High ability to retrieve relevant legal articles/cases |
| **Context Precision** | **0.800** | High signal-to-noise ratio in retrieved chunks |
| **Faithfulness** | **0.776** | Strong grounding in the provided legal text |
| **Answer Relevancy** | **0.546** | Aligning well with the user's intent |
| **Answer Correctness** | **0.549** | Reliable alignment with reference answers |

> **Note:** The Multi-Agent approach significantly outperformed Single-Agent modes in grounding, proving that supervisor-based routing enables more effective use of heterogeneous legal sources.

## 🛠 Tech Stack

* **LLM Inference:**
    * **OpenAI:** `GPT-4o-mini` (Selected for final system)
    * **Hugging Face:** `meta-llama/Llama-3.3-70B-Instruct` (Tested for comparison)
* **Embeddings:**
    * `text-embedding-ada-002` (OpenAI - 1536 dim)
    * `sentence-transformers/all-MiniLM-L6-v2` (Local - 384 dim)
* **Vector Database:** FAISS (Facebook AI Similarity Search)
* **Evaluation Framework:** RAGAS
* **Data Format:** Structured JSON (Civil Code Articles & Court Case Decisions)

## ⚙️ Methodology

### 1. Data Preprocessing & Embedding
The legal corpus resides locally in a structured JSON format, grouped by country and legal domain. We maintained separate FAISS indexes for each country/domain to support specialized retrieval.
* **Index.faiss:** Stores high-dimensional vector similarities.
* **Index.pkl:** Stores metadata and mappings to original text for context reconstruction.

### 2. System Architectures
We implemented and tested two main architectural flows:

* **Single-Agent RAG:**
    * **Standard Mode:** Direct retrieval and generation (optimized for speed).
    * **ReAct Mode:** Uses a reasoning-action-observation loop for better interpretability.
    * **Hybrid Legal Mode:** Combines semantic search with metadata filtering (jurisdiction/domain).

* **Multi-Agent RAG (Supervisor-Worker Pattern):**
    * **Supervisor Agent:** Interprets the query and routes it to the correct jurisdiction.
    * **Sub-Agents:** Country-specific agents that retrieve documents from their dedicated FAISS index and generate partial answers.

### 3. Pipeline Execution
1.  **User Query:** The system receives a legal question.
2.  **Routing/Reasoning:** The Supervisor determines if retrieval is required and selects the relevant domain.
3.  **Vector Search:** Relevant chunks are retrieved via FAISS.
4.  **Generation:** The LLM generates a grounded answer using the retrieved context.
5.  **Output:** Final answer provided with sources and metadata.
