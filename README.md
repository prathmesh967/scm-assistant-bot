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

https://cloud.flowiseai.com/chatbot/bd2c2b1f-8bdf-46fa-b549-d0d6d5cc9131

---

## Limitations

- The chatbot uses Retrieval-Augmented Generation (RAG), which retrieves only the most relevant records rather than the entire dataset.
- Questions requiring full-dataset analysis (totals, averages, counts, regional spend, rebate eligibility, and watch lists) may be incomplete if some relevant records are not retrieved.
- Token and context-window limits restrict how much data can be analyzed in a single query.
- Vector search performs well for supplier and policy lookups but is less effective for exact calculations across all 2,000 purchase-order records.

## Proposed Improvements

- Implement a hybrid architecture combining RAG with SQL/Pandas-based analytics for accurate calculations and aggregations.
- Add metadata filtering (Supplier, Region, Tier, Risk Level, etc.) to improve retrieval precision.
- Use separate retrievers for supplier performance data and governance policy documents.
- Combine semantic search with keyword search to improve coverage of structured data.
