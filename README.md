# RAG Q&A System — Ask Questions on Any File

Upload any PDF (including scanned), Excel, or CSV and ask questions about it in plain English. Built with ChromaDB, SentenceTransformers, and Groq's Llama 3.3 70B — runs 100% free on Google Colab.

---

## What it does

- Upload a PDF, CSV, or Excel — no code changes needed
- Automatically detects file type and extracts content
- Handles **scanned PDFs** using OCR fallback (pytesseract)
- Chunks content intelligently — row-by-row for tabular data, page-by-page for PDFs
- Stores chunks in a local vector database (ChromaDB)
- Ask any question in plain English and get a grounded answer
- Clears old data automatically when you switch files — no stale answers

---

## Demo

```
# On a resume (PDF)
Q: What skills does this person have?
A: Python (Pandas, NumPy, Scikit-Learn), SQL (Oracle 11g, MySQL), Power BI, ChromaDB, CrewAI...

# On sales data (Excel/CSV)
Q: Which region had the highest revenue?
A: South region with ₹95,000 on 2024-06-13.

# When info doesn't exist
Q: What is their phone number?
A: I don't have that information.
```

---

## How RAG works

```
Phase 1 — Store (once per file):
Your file → split into chunks → convert to numbers (embeddings) → store in ChromaDB

Phase 2 — Answer (every question):
Your question → embed → find similar chunks → send to LLM → grounded answer
```

The LLM only sees YOUR document — no hallucinations, no made-up answers.

---

## Quickstart

### Step 1 — Get a free Groq API key
1. Go to [console.groq.com](https://console.groq.com)
2. Sign up → API Keys → Create API Key
3. Copy the key

### Step 2 — Add key to Colab Secrets
1. Open notebook in Google Colab
2. Click the **key icon** in the left sidebar
3. Add new secret → Name: `GROQ_API_KEY` → paste your key
4. Toggle **Notebook access** ON

### Step 3 — Run the notebook
Open `rag_qa_bot.py` in Colab and run all cells top to bottom.

---

## File structure

```
rag-qa-system/
│
├── rag_qa_bot.py          # Main notebook — open this in Colab
├── README.md              # This file
├── requirements.txt       # Python dependencies
└── sample_data/
    └── sales_data.csv     # Sample file to test with
```

---

## Supported file types

| Type | Method | Notes |
|---|---|---|
| PDF (text) | PyMuPDF direct extraction | Fast, accurate |
| PDF (scanned) | OCR via pytesseract | Auto-detected, ~5s per page |
| CSV | Row-by-row with headers | Headers always attached to values |
| Excel (.xlsx) | Row-by-row with headers | Headers always attached to values |

### Why row-by-row chunking matters for Excel/CSV

Most RAG tutorials chunk by word count. That breaks tabular data:

```
❌ Word count chunking:   "South 85000 320 North 62000 210"
✅ Row-by-row chunking:   "Region: South | Revenue: 85000 | Units Sold: 320"
```

When headers stay attached to values, the LLM always knows what each number means. Accuracy jumps significantly on real data.

---

## Tech stack

- **Python** — pandas, PyMuPDF
- **ChromaDB** — local vector database, no server needed
- **SentenceTransformers** — free local embedding model (all-MiniLM-L6-v2)
- **Groq API** — free LLM inference (Llama 3.3 70B)
- **pytesseract** — OCR for scanned PDFs
- **Google Colab** — free cloud runtime

---

## Known limitations

- Scanned PDFs take ~2-3 minutes for 25 pages — progress shown per page
- Very large Excel files (10,000+ rows) may be slow to embed
- Groq free tier has daily limits — switch to rule-based fallback if exceeded

---

## Skills demonstrated

- Retrieval Augmented Generation (RAG) from scratch
- Vector embeddings and similarity search
- OCR for scanned document handling
- Multi-format file ingestion (PDF, Excel, CSV)
- LLM API integration (Groq)
- Google Colab workflow

---

## License

MIT — free to use, modify, and share.
