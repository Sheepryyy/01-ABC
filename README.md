# 01-ABC

# 🚨 Suspicious Activity Report (SAR) Generator Using GenAI

This project automates the generation of Suspicious Activity Reports (SARs) for the Agricultural Bank of China, New York Branch. It combines a PostgreSQL database, document-based Retrieval-Augmented Generation (RAG), and a Large Language Model (LLM) to produce structured SAR narratives.

---

## 🖼️ 1. Visual Overview

- Visual representations of the SAR generation pipeline, metadata structure, and relational database schemas.
![image](https://github.com/user-attachments/assets/92e2f83d-aa6c-4914-ae3f-e882e946a628)

---

## ⚙️ 2. Configuration Steps

- This section contains all related configuration steps from database creation to querying the data, creating the prompt and any function required for greater automation of the SAR Creation process.

### 📌 2.1 Database Creation

- PostgreSQL schema with 4 tables: `Customer`, `Account`, `Transaction`, `Alert`
![image](https://github.com/user-attachments/assets/0f0130bd-5da1-4e63-88a5-61b771e5c9cf)
- Use `psycopg2` to set up the connection to PostgreSQL
- The four tables to be created will be connected to each other as per the ERD Diagram below
![image](https://github.com/user-attachments/assets/1fe69a20-120e-4f93-84ee-50cb69fa60c0)
- Creating the four tables and importing four local CSV files into Database

### 🔍 2.2 RAG Setup: Alert Narratives and Guidelines (Section 4)

- Load 3 fake SAR narratives and a guidelines document from `.docx` files:
  - `A-1 Alert Narrative.docx`
  - `A-2 Fake Alert Narrative Fixed.docx`
  - `A-5 Fake Alert Narrative.docx`
  - `Suspicious Activity Guidelines.docx`
- Split each document into text chunks (paragraphs) using `docx2txt`, keeping only those with more than 50 characters.
- Generate embeddings with `HuggingFaceEmbeddings`(model name = "all-MiniLM-L6-v2")
- Store them in a `FAISS` vector store using LangChain.

### 🧠 2.3 Prompt Generation

- A structured prompt template is defined for use by the LLM to generate each SAR narrative.
- Placeholders in {braces} are dynamically filled using data queried from the database.
- Two versions of the prompt are used: one for Individual clients and another for Business clients, reflecting different available data (e.g., DOB, SSN, address).
- Construct dynamic prompts using information retrieved from both:
  - SQL queries (for entity information)
  - FAISS (for similar SAR examples)

### 🔗 2.4 Query Functions: Building Prompt Variables

- To generate customized SAR narratives, two query functions are used—one for individual clients and one for business clients.
- Each function:
  - Queries the Alert, Transaction, Customer, and Account tables to extract relevant data.
  - Builds a transaction summary (including direction and date of wire transfers).
  - Assembles KYC details and summary statistics (transaction count, amount, etc.).
  - Retrieves similar case examples from the FAISS index based on the transaction summary.
  - Returns a dictionary of variables to fill the prompt's placeholders.
- A separate utility extracts the Customer ID from .docx files using regular expressions.

### 🤖 2.5 LLM Set Up

- Use AWS configure to link AWS bedrock(Access Key ID, Secret Access Key and Region)
- AWS Bedrock models (e.g., Deepseek R1 70B, LLaMA 3.3 70B)

---

## 🧾 3. SAR Generation

- In order to analyze the capability of LLMs in generating SARs, the code below will create 3 SAR Narratives based on the 3 SAR Alert Narratives provided by the client.
- For each of the available SAR Alert Narratives, output will be provided from two models, Llama 3.3 and DeepSeek R-1, on 3 three different temperature (0.3, 0.6, 0.9) settings.
- This will allow us to then guage which temperature is ideal per model and by using the newly created grading rubric we will be able to asses with model performs best.

### 📝 Alert Narrative 1

### 📝 Alert Narrative 2

### 📝 Alert Narrative 3

---
