# vidhi-vichara

# ⚖️ Vidhi-Vichara
### AI That Aligns Rules with Law
 
> A not-for-profit research prototype built at IIT Jodhpur that automatically detects when executive actions (circulars, notifications, guidelines) deviate from their parent legislation.
 
---
 
## What It Does
 
Legislatures pass laws. Executives issue rules. Over time, those rules can quietly drift away from legislative intent — tightening deadlines, expanding restrictions, or contradicting fundamental rights.
 
**Vidhi-Vichara** takes any proposed rule or executive notification, retrieves the most relevant sections from across the Indian legal corpus, and outputs a structured deviation report:
 
- **Deviation Score** (1–10): how far the rule strays from established law
- **Relevant Act**: the specific Indian Act that governs this area
- **Location of Deviation**: exactly what part of the rule is the problem
- **Explanation**: why it clashes with established law
- **Suggested Fix**: how to rephrase it to align correctly
---
 
## Tech Stack
 
| Component | Tool |
|---|---|
| Document Parsing | Sarvam AI Document Intelligence + PyMuPDF |
| Embeddings | `BAAI/bge-small-en-v1.5` (HuggingFace) |
| Vector Database | Qdrant (local) |
| LLM | Llama 3.3 70B via Groq API |
| Orchestration | LangChain LCEL |
| Data Source | India Code (indiacode.nic.in) |
 
---
 
## Pipeline
 
```
India Code (6,746 Acts scraped)
        ↓
PDF Download + Text Extraction
(PyMuPDF for digital, Sarvam for scanned)
        ↓
Chunking + Embedding → Qdrant Vector DB
        ↓
RAG Chain (retrieve top-5 relevant legal chunks)
        ↓
Llama 3.3 70B → Structured Deviation Report
        ↓
Live Widget UI (Green / Amber / Red signal)
```
 
---
 
## Results
 
| Test Rule | Expected | Got | Cited Act |
|---|---|---|---|
| AGM within 6 months, 21-day notice | 🟢 1-3 | 🟢 1 | Companies Act, 2013 |
| RTI response within 60 days | 🟡 4-6 | 🟢 1 | RTI Act, 2005 |
| Terminate female staff after 12 weeks maternity | 🔴 7-9 | 🔴 8 | Maternity Benefit Act, 1961 |
| 14-day detention without magistrate | 🔴 10 | 🔴 8 | CrPC, 1973 (S.57 & 167) |
 
---
 
## Setup
 
### Prerequisites
- Python 3.10+
- Kaggle or Google Colab (GPU recommended for embedding)
- [Sarvam AI API Key](https://sarvam.ai)
- [Groq API Key](https://console.groq.com)
### Run
 
1. Open `vidhi-vichara.ipynb` in Kaggle or Colab
2. Run Cell 1 to install dependencies
3. Enter your Sarvam and Groq API keys when prompted
4. Run all cells in order — the pipeline handles everything automatically
5. Use the live widget at the bottom to audit any rule
> **Note:** Cells 4–8 (scraping, downloading, embedding) take ~2 hours total on first run. After that, the database persists and you can skip straight to the RAG chain cell.
 
---
 
## Known Limitation
 
India Code's server rate-limits bulk requests, so scraping captures ~6,700 of the estimated 10,000+ Acts. Their OAI-PMH and REST APIs return 403. We are working on getting an official data export from the Legislative Department for complete corpus coverage.
 
---
 
## Project Context
 
Built as part of a research collaboration with **Prof. Venkat Ram Reddy Ganuthula**, IIT Jodhpur (Behavioral Science × Decision-making × AI Governance).
 
The vision: an open API that any government department can use as a compliance checkpoint before gazette publication — ensuring every executive action stays faithfully connected to legislative intent.
 
---
 
## Acknowledgements
 
- [India Code](https://indiacode.nic.in) — Legislative Department, Ministry of Law & Justice
- [Sarvam AI](https://sarvam.ai) — Document Intelligence API
- [Groq](https://groq.com) — LLM inference
- [Qdrant](https://qdrant.tech) — Vector database
---
 
*Vidhi (विधि) = Law | Vichara (विचार) = Thought/Deliberation*
