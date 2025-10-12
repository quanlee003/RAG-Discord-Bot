# RAG Discord Bot


⚠️ **Important Notice – API Deprecated**
The original chatbot was built using Cohere's older Embed + Rerank + Chat endpoints. As of 2025, this API version is no longer supported, so **the bot currently does not function**.  
A future update will migrate the system to a new provider (OpenAI / Groq / Cohere v3 / Ollama) to restore functionality.

---
A Retrieval-Augmented Generation (RAG) powered Discord chatbot that retrieves lecture content from local documents and generates accurate responses inside Discord, minimizing hallucinations by grounding answers in stored knowledge.

---

## 🎯 Overview

This chatbot is designed for **students and educators**. It can:

- Load PDF, DOCX, and TXT lecture materials from the `content/` directory.
- Embed and index documents using **Cohere Embed API + HNSWlib**.
- Retrieve the most relevant text chunks via **semantic search + reranking**.
- Generate natural responses using **Cohere Chat API**.
- Operate directly inside Discord via mention-based interaction.
- Update knowledge base with `/restart` without restarting the server manually.

---

## ⚙️ How It Works (RAG Pipeline)

```
User Message
     │
     ▼
┌────────────────────────────────────────────┐
│ 1. Query Embedding (Cohere Embed API)      │
└────────────────────────────────────────────┘
     │
     ▼
┌────────────────────────────────────────────┐
│ 2. Vector Search (HNSWlib - KNN similarity)│
└────────────────────────────────────────────┘
     │ Top-K similar chunks
     ▼
┌────────────────────────────────────────────┐
│ 3. Rerank Results (Cohere Rerank API)      │
└────────────────────────────────────────────┘
     │ Best match selected
     ▼
┌────────────────────────────────────────────┐
│ 4. Response Generation (Cohere Chat API)   │
└────────────────────────────────────────────┘
     │
     ▼
Bot Replies in Discord (No Hallucination)
```

---

## 📁 Project Structure

```
rag-discord-bot/
│── main.py                # Discord event handler and bot runtime
│── chatbot.py             # Core RAG logic (load, embed, retrieve, generate)
│── request_queue.py       # Queue system for async processing
│── document_url.py        # (If used) Linking external sources
│── requirements.txt       # Required dependencies
│── .env                   # API keys and bot tokens
│── README.md
│
└── content/               # Knowledge base (lecture materials)
    ├── week1/
    ├── week2/
    └── week3/
```

---

## 📌 Requirements

### 1️⃣ Install dependencies
```bash
pip install -r requirements.txt
```

### 2️⃣ Configure `.env`
```
DISCORD_BOT_TOKEN=YOUR_DISCORD_BOT_TOKEN
COHERE_API_KEY=YOUR_COHERE_API_KEY
```

### 3️⃣ Run the bot
```bash
python main.py
```

---

## 🎮 Usage in Discord

| Action | How to Use |
|--------|-----------|
| Ask a question | `@BotName your question here` |
| Reload documents after adding new files | `/restart` |
| Add external source (future feature) | `/addsource <url>` *(not yet active)* |

---

## 📂 Adding New Lecture Files

1. Place your **PDF, DOCX, or TXT** files inside any folder under `/content/`
2. Example:
```
content/
 ├── week1/lecture1.pdf
 ├── week2/notes.docx
 └── week3/summary.txt
```
3. In Discord, run:
```
/restart
```
→ The bot will re-index and rebuild its vector store.

---

## 🧠 Tech Breakdown

| Component | Library / API Used |
|----------|------------------|
| Document Loading & Chunking | `unstructured` |
| Embedding | Cohere `embed` endpoint |
| Index / ANN Search | `HNSWlib` |
| Reranking | Cohere `rerank` |
| Answer Generation | Cohere `chat` |
| Discord Integration | `discord.py` |
| Queue Management | `request_queue.py` |

---

## 🔮 Future Enhancements (Optional Ideas)

- ✅ `/addsource` to index external URLs dynamically
- ✅ Web dashboard to view indexed content
- 🔲 Support for **voice input & output**
- 🔲 Web UI for uploading files instead of folder-based
- 🔲 Database instead of local in-memory HNSW index

---

## ✅ Summary

- Drop files → `/restart` → Ask with `@BotName`.
- Uses **Cohere + HNSWlib** for high-speed retrieval.
- Fully grounded RAG architecture → prevents hallucination.
- Designed for **education workflows on Discord**.
