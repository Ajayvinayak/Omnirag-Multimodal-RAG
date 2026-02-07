# OmniRAG – Multimodal Retrieval‑Augmented Generation System

OmniRAG is a **pure Retrieval‑Augmented Generation (RAG)** backend system that allows users to ingest documents (PDF, CSV) and ask questions that are answered **strictly from the ingested data**.

This project is intentionally designed to **avoid hallucinations** and behave like an **enterprise‑grade knowledge assistant**, not a general‑purpose chatbot.

---

## 🚀 Key Features

* 📄 **PDF Ingestion** – Extracts and embeds document text
* 📊 **CSV Ingestion** –

  * Column‑level semantic embeddings
  * Row‑level embeddings (first 500 rows)
* 🔎 **Vector Search** using embeddings
* 🧠 **LLM‑based Answer Generation** grounded only in retrieved context
* 🧾 **Source Attribution** for every answer
* 🛑 **No Hallucination Guarantee** – answers are refused if data is missing

---

## 🏗️ System Architecture

```
User Question
     ↓
Query Embedding
     ↓
Vector Database (Pinecone)
     ↓
Top‑K Relevant Chunks
     ↓
LLM (Context‑Restricted)
     ↓
Final Answer + Sources
```

> ⚠️ The LLM is **not allowed to answer using its own knowledge**.

---

## 📁 Project Structure

```
backend/
│── main.py                 # FastAPI app
│
├── ingest/
│   ├── pdf_ingest.py       # PDF ingestion
│   ├── csv_ingest.py       # CSV ingestion
│
├── rag/
│   └── query.py            # Retrieval + answer pipeline
│
├── core/
│   ├── embeddings.py       # Text → vector embeddings
│   ├── pinecone_client.py  # Vector DB connection
│
├── llm.py                  # LLM wrapper (Groq)
└── venv/
```

---

## ⚙️ Setup Instructions

### 1️⃣ Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate
```

### 2️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

### 3️⃣ Environment Variables

```bash
export GROQ_API_KEY=your_key_here
export PINECONE_API_KEY=your_key_here
```

---

## ▶️ Run the Server

```bash
uvicorn main:app --host 0.0.0.0 --port 8001 --reload
```

Health check:

```bash
GET http://localhost:8001/health
```

---

## 📥 Data Ingestion

### 📄 PDF Ingestion

```bash
curl -X POST "http://localhost:8001/ingest/pdf?path=/absolute/path/to/file.pdf"
```

### 📊 CSV Ingestion

```bash
curl -X POST "http://localhost:8001/ingest/csv?path=/absolute/path/to/file.csv"
```

What CSV ingestion does:

* Column‑level semantic understanding
* Row‑level data grounding
* Metadata tagging (`type = column | row`)

---

## ❓ Querying the System

```bash
curl -X POST http://localhost:8001/query \
-H "Content-Type: application/json" \
-d '{
  "question": "Which coffee products generate the highest revenue?",
  "top_k": 5
}'
```

### Example Response

```json
{
  "question": "Which coffee products generate the highest revenue?",
  "answer": "Espresso generates the highest revenue.",
  "sources": ["./sales.csv"]
}
```

---

## 🛑 Hallucination Control

If the answer is **not present in the ingested documents**, the system responds with:

> "The answer is not available in the provided data."

This makes OmniRAG suitable for:

* Enterprise analytics
* Compliance systems
* Knowledge bases
* Academic projects

---

## 🎓 Academic Use (Final‑Year Project Ready)

This project demonstrates:

* Information Retrieval
* Semantic Search
* Embeddings
* Vector Databases
* LLM grounding
* Responsible AI (No hallucinations)

---

## 🔮 Future Enhancements (Optional)

* UI dashboard
* Authentication
* Chunk scoring threshold tuning
* Multi‑file querying
* Table‑aware reasoning

---

## 👨‍💻 Author

**Ajay Vinayak**
Final‑Year B.Tech – Computer Science (Big Data Analytics)

---

✅ This project is intentionally scoped to remain **robust, explainable, and interview‑safe**.
# Omnirag-Multimodal-RAG
A production-ready multimodal RAG system that ingests documents and datasets, stores embeddings in Pinecone, and generates grounded answers using Grok.
