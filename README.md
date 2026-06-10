# SCM Assistant Bot

A Retrieval-Augmented Generation (RAG) chatbot built using Flowise Cloud to answer supplier network, procurement, compliance, risk, and governance policy questions.

## Project Overview

SCM Assistant is designed to help users query supplier performance data and supply chain governance policies using natural language. The chatbot combines semantic search with a Large Language Model (LLM) to retrieve relevant supplier records and policy sections before generating responses.

## Technology Stack

### Platform

* Flowise Cloud

### LLM

* Groq
* Model: `llama-3.3-70b-versatile`
* Temperature: `0`

### Embeddings Model

* HuggingFace Inference
* Model: `sentence-transformers/all-MiniLM-L6-v2`

### Vector Store

* In-Memory Vector Store

### Memory

* Buffer Memory

---

## Knowledge Base

### Files Used

1. `supplier_performance_data.csv`
2. `SupplyChain_Governance_Policy_v3.2.pdf`

No modifications were made to the provided source files.

---

## Chunking Experiments

### Configuration 1

#### CSV

* Recursive Character Text Splitter
* Chunk Size: 2000
* Chunk Overlap: 200

#### PDF

* Recursive Character Text Splitter
* Chunk Size: 2000
* Chunk Overlap: 200

#### Results

* CSV Chunks: 2000
* PDF Chunks: 13

Observation:

* Larger chunks preserved more context but reduced retrieval precision for supplier-specific queries.

---

### Configuration 2 (Final)

#### CSV

* Recursive Character Text Splitter
* Chunk Size: 1000
* Chunk Overlap: 100

#### PDF

* Recursive Character Text Splitter
* Chunk Size: 1000
* Chunk Overlap: 100

#### Results

* CSV Chunks: 2000
* PDF Chunks: 19

Observation:

* Improved retrieval quality for supplier records and policy sections while staying within token limits.

---

## Chatflow Architecture

Document Store
→ HuggingFace Embeddings
→ In-Memory Vector Store
→ Memory Retriever
→ Conversational Retrieval QA Chain
→ Groq LLM

Additional Components:

* Buffer Memory
* Retrieval-Augmented Generation (RAG)

---

## Public Chatbot URL

Add your deployed Flowise chatbot URL here:

https://cloud.flowiseai.com/chatbot/115a13cc-c710-46b0-944f-73ce54b7ea9b

---

## Required Evaluation Questions

### Q1

**Question**

Which Tier-3 suppliers have an active disruption flag, and what response level applies per policy?

**Answer**

11 Tier-3 suppliers: Dravex Components India, Plataforma Metales SA, Maghreb Castworks, Helios Pack Greece, Cerromax Mineria, Orinoco Pack SAPI, Quetzal Textiles, Sibertek Molding, Archipelago PCB Corp, Varna Electronics EAD, Deltaforge Vietnam.

All are High Risk and require **Level 3 Activate** response per Policy §9, including CPO escalation and alternate supplier activation at a minimum of 40% volume.

---

### Q2

**Question**

Which suppliers qualify for the annual Volume Rebate Program and how many?

**Answer**

19 suppliers qualify:

* Borealis Composites
* Crestline Chemical Supply
* Fenwick Alloy Solutions
* Hanguk Circuit Works
* Hokkaido Alloy Tech
* Krauss-Polymex GmbH
* Lakeshore Components
* Lumivex Semiconductor NL
* Maplewood Polymer Corp
* Norbec Alloy Works
* Nordloom Finland Oy
* Orrentek Precision Mfg
* Ostwind Composites AG
* PrecisionForge Taiyuan
* Solveig Eco Packaging
* Straits Packaging Hub
* Tasman Circuit Boards
* Toreval Electronics
* Valdoro Special Alloys

Criteria (Policy §4.2):

* Tier-1
* OTD ≥ 93%
* Defect Rate < 0.5%
* Sustainability Score ≥ 85

---

### Q3

**Question**

Which region has the highest total PO value, and does it breach the concentration limit?

**Answer**

EMEA has the highest total PO value at **$193,987,179.91**, representing approximately **48.5%** of total spend (**$399,563,494.10**).

This breaches the **45% regional concentration cap** defined in Policy §5.3 and requires a Diversification Plan within 60 days.

---

### Q4

**Question**

Which suppliers are on Supplier Watch List (SWL) status and what does it restrict?

**Answer**

11 suppliers qualify for Supplier Watch List (Compliance Score < 60):

* Deltaforge Vietnam
* Maghreb Castworks
* Helios Pack Greece
* Cerromax Mineria
* Orinoco Pack SAPI
* Varna Electronics EAD
* Quetzal Textiles
* Plataforma Metales SA
* Archipelago PCB Corp
* Dravex Components India
* Sibertek Molding

Policy §3.4 restricts new PO issuance to **20% of prior quarter volume**.

---

### Q5

**Question**

Which product category has the highest average defect rate and does it exceed the Tier-2 limit?

**Answer**

Mechanical Components has the highest average defect rate at **2.12%** across **360 purchase orders**.

The Tier-2 defect-rate ceiling is **2.50%** (Policy §3.2).

Result:

* No breach
* Approaching threshold

---

## Screenshots

Screenshots demonstrating setup and testing are available in:

```text
/screenshots
```

Suggested screenshots:

1. Document Store Creation
2. CSV Upload
3. PDF Upload
4. Chunk Configuration
5. Embeddings Configuration
6. Vector Store Configuration
7. Chatflow Architecture
8. Successful Upsert
9. Public Chatbot Deployment
10. Q1 Result
11. Q2 Result
12. Q3 Result
13. Q4 Result
14. Q5 Result

---

## Limitations

* Vector RAG retrieval may not always retrieve all relevant records for large aggregate calculations.
* Aggregate supplier analytics are more challenging than supplier-specific lookups because retrieval operates on document chunks rather than full-dataset scans.
* Results depend on chunking strategy, retrieval quality, and context window limits.

---

## Future Improvements

If additional tooling were permitted, I would improve the system by:

1. Adding a hybrid SQL/DataFrame retrieval layer for exact aggregations.
2. Using metadata-aware filtering for supplier, region, and tier fields.
3. Adding reranking for improved retrieval precision.
4. Creating separate retrievers for policy documents and supplier records.
5. Using hybrid keyword + semantic search.
6. Adding citation support and source highlighting.
7. Implementing evaluation metrics for retrieval quality and answer correctness.

---

## Repository Structure

```text
scm-assistant-bot/
│
├── scm_assistant.json
├── README.md
├── .gitignore
│
└── screenshots/
    ├── 01-document-store.png
    ├── 02-csv-upload.png
    ├── 03-pdf-upload.png
    ├── ...
```
