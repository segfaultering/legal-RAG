# AI Lawyer — RAG with DeepSeek R1

An AI-powered legal chatbot that uses Retrieval-Augmented Generation (RAG) to answer questions about uploaded legal documents. By grounding responses in actual document content, it reduces hallucinations and provides reliable, context-aware legal insights.

---

## Features

- Upload and analyze legal documents (PDF)
- Document summarization
- Chat interface for asking legal questions
- Retrieval of relevant document sections via FAISS vector search
- AI-generated responses powered by DeepSeek R1
- Downloadable PDF report of the conversation

---

## Technologies

| Layer | Technology |
|---|---|
| LLM | DeepSeek R1 Distill Llama 70B (via Groq API) |
| Embeddings | sentence-transformers/all-MiniLM-L6-v2 |
| Vector Database | FAISS (CPU) |
| Orchestration | LangChain |
| PDF Processing | pdfplumber, pypdfium2 |
| Frontend | Streamlit |
| Report Generation | ReportLab |

---

## Project Structure

```
legal-rag/
├── frontend.py              # Streamlit UI (primary entry point)
├── main.py                  # Alternative Streamlit entry point (Ollama-based)
├── rag_pipeline.py          # RAG pipeline: retrieval, answering, summarization, report generation
├── vector_database.py       # Document loading, chunking, embedding, FAISS indexing
├── requirements.txt         # Pinned Python dependencies
├── pdfs/                   # Directory for uploaded PDF documents
├── vectorstore/
│   └── db_faiss/           # Pre-built FAISS vector index
│       ├── index.faiss
│       └── index.pkl
├── utils/                   # Demo screenshots
└── .devcontainer/
    └── devcontainer.json   # VS Code Dev Container configuration
```

---

## Installation

### Prerequisites

- Python 3.11+
- A [Groq API key](https://console.groq.com/)

### Steps

1. Clone the repository:

   ```bash
   git clone https://github.com/AbhaySingh71/AI-Lawyer-RAG-with-Deepseek.git
   cd AI-Lawyer-RAG-with-Deepseek
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv venv
   source venv/bin/activate  # macOS/Linux
   venv\Scripts\activate     # Windows
   ```

3. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

4. Set your Groq API key as an environment variable:

   ```bash
   export GROQ_API_KEY="your-api-key-here"  # macOS/Linux
   set GROQ_API_KEY="your-api-key-here"     # Windows
   ```

   Alternatively, create a `.env` file in the project root:

   ```
   GROQ_API_KEY=your-api-key-here
   ```

---

## Usage

Run the Streamlit application:

```bash
streamlit run frontend.py
```

The app will open in your browser at `http://localhost:8501`.

### Workflow

1. **Upload a PDF** — Click the file uploader and select a legal document.
2. **Summarize** — Click "Summarize Document" to get a concise overview.
3. **Ask questions** — Type your legal question in the text area and click "Ask AI Lawyer".
4. **Download report** — After a conversation, click "Download Report" to export the Q&A history as a PDF.

---

## How It Works

1. **Upload** — The PDF is saved to the `pdfs/` directory.
2. **Extract** — `pdfplumber` extracts text from the document.
3. **Chunk** — Text is split into 1000-character chunks with 200-character overlap using `RecursiveCharacterTextSplitter`.
4. **Embed** — Chunks are embedded using `sentence-transformers/all-MiniLM-L6-v2`.
5. **Index** — Embeddings are stored in a FAISS vector index and saved to `vectorstore/db_faiss/`.
6. **Retrieve** — On query, FAISS performs similarity search (k=5) and filters results by source filename.
7. **Generate** — Retrieved context is passed to DeepSeek R1 (via Groq) with a prompt that enforces strict grounding in the provided context.
8. **Report** — The conversation history can be exported as a formatted PDF via ReportLab.

---

## Deployment

### Streamlit Cloud

1. Push the code to a GitHub repository.
2. Go to [Streamlit Cloud](https://share.streamlit.io/) and click "Deploy a new app".
3. Set `GROQ_API_KEY` in Streamlit Secrets.
4. Click **Deploy**.

### Dev Container (VS Code)

The included `.devcontainer/devcontainer.json` provides a pre-configured development environment. Open the project in VS Code and select "Reopen in Container" to use it.

---

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `GROQ_API_KEY` | Yes | API key for Groq (used by LangChain's `ChatGroq`) |

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
