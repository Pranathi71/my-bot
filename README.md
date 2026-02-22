# 🤖 Smart Resume AI Agent

### A RAG-Based Resume Question Answering System

## 📌 Introduction

**Smart Resume AI Agent** is a **Retrieval-Augmented Generation (RAG)** powered web application that allows users to ask **natural language questions** about their resume PDF and receive **accurate, resume-grounded answers**.

Instead of guessing or hallucinating, the system strictly answers **only from the uploaded resume**, making it reliable, explainable, and production-ready.

This project demonstrates how modern NLP techniques can be applied to build an **intelligent resume assistant** using open-source tools.

---

## 🚀 Key Features

* 📄 Upload any resume PDF
* 🔍 Ask questions like:

  * *“What are my technical skills?”*
  * *“Describe my projects”*
  * *“What certifications do I have?”*
* 🧠 AI answers are **strictly based on resume content**
* ⚡ Fast semantic search using vector embeddings
* 🧩 Structured resume understanding (section-aware)

---

## 🧠 What is Retrieval-Augmented Generation (RAG)?

**Retrieval-Augmented Generation (RAG)** is an AI architecture that combines:

1. **Information Retrieval** (searching relevant documents)
2. **Text Generation** (producing a natural language answer)

### Why Not Just Use an LLM?

Traditional LLMs often:

* Hallucinate information
* Answer beyond the provided data
* Lack grounding in user-specific documents

### How RAG Solves This

RAG introduces a retrieval step before generation:

1. 🔎 Retrieve relevant resume sections
2. 🧾 Inject them into the prompt
3. ✍️ Generate an answer using only that context

✅ This ensures factual, explainable, and personalized responses.

---

## 🏗️ System Architecture

```
User Question
     ↓
Streamlit Interface
     ↓
FAISS Resume Retriever
     ↓
Relevant Resume Chunks
     ↓
Prompt Construction
     ↓
Flan-T5 Language Model
     ↓
Final Answer
```

---

## 🔄 End-to-End Workflow

### 1️⃣ Resume Upload & Text Extraction

* Resume PDFs are uploaded through the UI
* Text is extracted using `PDFMinerLoader`

**Why PDFMiner?**

* Preserves resume layout
* Handles multi-column resumes well
* Produces cleaner text than OCR-based tools

---

### 2️⃣ Resume Text Cleaning

Raw PDF text often contains noise.

Cleaning steps include:

* Removing extra new lines
* Fixing spacing issues
* Normalizing Unicode characters

🎯 **Goal:** Improve embedding quality and retrieval accuracy.

---

### 3️⃣ Section-Based Resume Parsing

Instead of random chunking, the resume is split using common headers:

```
    "EDUCATION",
    "INTERNSHIP",
    "PROJECTS",
    "SKILLS",
    "ACHIEVEMENTS",
    "CERTIFICATES",
    "EXTRA-CURRICULAR ACTIVITIES"
```

**Why section-based splitting?**

* Preserves semantic meaning
* Maintains resume structure
* Improves relevance during search

---

### 4️⃣ Smart Resume Chunking

Each section is further split into:

* ~350 character chunks
* With section metadata attached

✔ Better semantic embeddings
✔ Faster and more accurate retrieval

---

### 5️⃣ Embedding Generation

Each resume chunk is converted into vectors using:

**Model:** `BAAI/bge-large-en-v1.5`

**Why this model?**

* High semantic similarity accuracy
* Strong performance in retrieval tasks
* Industry-grade embedding quality

---

### 6️⃣ Vector Database (FAISS)

Embeddings are stored in **FAISS**
(Facebook AI Similarity Search)

**Benefits:**

* Extremely fast similarity search
* Memory-efficient
* Ideal for RAG pipelines

---

### 7️⃣ Resume Retriever

Retriever configuration:

```
Top-K = 6
```

For every question:

* The top 6 most relevant resume chunks are fetched
* These chunks form the **context window**

---

### 8️⃣ Prompt Engineering (Hallucination Control)

The prompt enforces strict rules:

* Answer **only from resume content**
* If information is missing → return
  **“Not available in resume”**
* Keep answers short and factual

🚫 Prevents guessing
🚫 Prevents external knowledge leakage

---

### 9️⃣ Answer Generation

The final response is generated using:

**Model:** `google/flan-t5-large`
(by **Google**)

**Why Flan-T5?**

* Instruction-tuned
* Excellent for Q&A tasks
* Lightweight and cost-effective
* Runs locally without paid APIs

**Configuration:**

* Max tokens: 256
* Temperature: 0.0 (deterministic output)

---

### 🔟 Frontend (Streamlit)

The UI is built using **Streamlit**

**Features:**

* Chat-style interface
* Resume chunk inspection
* Chat history
* Loading spinner for better UX

---

## 🧩 Tech Stack

| Tool / Library           | Purpose                       |
| ------------------------ | ----------------------------- |
| Streamlit                | Web Interface                 |
| LangChain Community      | Document loaders & schemas    |
| PDFMiner                 | Resume text extraction        |
| HuggingFace Transformers | Answer generation (Flan-T5)   |
| HuggingFace Embeddings   | Vector embedding generation   |
| FAISS                    | Vector similarity search      |
| Regex (`re`)             | Text cleaning & preprocessing |

---

## 🎯 Why This Project Stands Out

* ✅ Fully resume-grounded answers
* ✅ No hallucinations
* ✅ Real-world RAG architecture
* ✅ Recruiter-friendly and explainable
* ✅ Open-source & local-first

---

## 👩‍💻 Author

**Built by:** *Pranathi*
