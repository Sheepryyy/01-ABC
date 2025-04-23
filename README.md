# 01-ABC
### Develop environment
**Step 1:** Download ollama using the following link - https://ollama.com/download

**Step 2:** Complete ollama set-up.

**Step 3:** Use the command prompt(eg.Anaconda Prompt) to download Deepseek and Llama3 using the code below:
  * `ollama run deepseek-r1:7b`
  * `ollama run deepseek-llm:7b`
  * `ollama run llama3.1:8b`
  * `ollama run llama3:8b`

**Step 4:** Use the command prompt to install Open WebUI using the code below:
  * `pip install open-webui`
  * `open-webui serve`

# 🚨 Suspicious Activity Report (SAR) Generator Using GenAI

This project automates the generation of Suspicious Activity Reports (SARs) for the Agricultural Bank of China, New York Branch. It combines a PostgreSQL database, document-based Retrieval-Augmented Generation (RAG), and a Large Language Model (LLM) to produce structured SAR narratives.

---

## 🖼️ 1. Visual Overview

- Visual representations of the SAR generation pipeline, metadata structure, and relational database schemas.
- Files used: `ABC Flow Database.png`, `Metadata Improvement.pdf`, `Exhibit 2 Database.png`

---

## ⚙️ 2. Configuration Steps

### 📌 2.1 Database Creation

- PostgreSQL schema with 4 tables: `Customer`, `Account`, `Transaction`, `Alert`
- Use `psycopg2` to connect and populate the tables with synthetic data.

### 🔍 2.2 RAG Setup: Alert Narratives and Guidelines (Section 4)

- Load 3 sample `.docx` alert narratives as training references.
- Clean and tokenize the narratives using `docx2txt` and `re`.
- Generate embeddings with `HuggingFaceEmbeddings`
- Store them in a `FAISS` vector store using LangChain.

### 🧠 2.3 Prompt Generation

- Construct dynamic prompts using information retrieved from both:
  - SQL queries (for entity information)
  - FAISS (for similar SAR examples)

### 🔗 2.4 Query Functions

- Define helper functions to:
  - Query the PostgreSQL database for a given `alert_id`
  - Retrieve top-K relevant context passages from FAISS

### 🤖 2.5 LLM Set Up

- Option to use:
  - Open-source local LLM (e.g., LLaMA)
  - AWS Bedrock endpoint (e.g., Claude, LLaMA 3 70B)
- Define `invoke_model()` function for model calls

---

## 🧾 3. SAR Generation

Each SAR is generated based on real alert data and contextual examples.

### 📝 Alert Narrative 1
- Full pipeline run using first example

### 📝 Alert Narrative 2
- Highlighting improved accuracy in generation

### 📝 Alert Narrative 3
- Additional testing for generalizability and hallucination checks

---

## 🚀 How to Run

1. Install requirements
   ```bash
   pip install -r requirements.txt
