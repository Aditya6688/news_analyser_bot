📰 News Research Bot

An AI-powered assistant that makes financial and stock-market news analysis effortless. Paste a few article URLs, build semantic embeddings, and ask questions with cited sources — all from a simple Streamlit app.

---

## Table of Contents
- **Features**
- **Prerequisites**
- **Quick Start**
- **Configuration**
- **How It Works**
- **Troubleshooting**
- **Project Structure**

---

## Features
- **URL input**: Enter up to 3 news article URLs from the sidebar.
- **Content extraction**: Fetches and parses content via LangChain’s `UnstructuredURLLoader`.
- **Fast retrieval**: Creates OpenAI embeddings and stores them in a local FAISS index for vector search.
- **Q&A with sources**: Ask questions and get answers with referenced sources.
- **Reusable index**: Saves the index to `faiss_store_openai.pkl` for faster subsequent queries.

---

## Prerequisites
- Python 3.10+ recommended
- An OpenAI API key

---

## Quick Start
1) Clone or download this project, then open a terminal at:
```
news_research_tool/
```

2) (Optional) Create and activate a virtual environment.

Windows (PowerShell):
```bash
python -m venv .venv
.venv\\Scripts\\Activate.ps1
```

macOS/Linux:
```bash
python -m venv .venv
source .venv/bin/activate
```

3) Install dependencies:
```bash
pip install -r requirements.txt
```

4) Add your OpenAI API key. Create a `.env` file next to `main.py` with:
```bash
OPENAI_API_KEY=your_api_key_here
```

5) Run the app:
```bash
streamlit run main.py
```

6) In your browser:
- Enter up to three article URLs in the left sidebar
- Click "Process URLs" to fetch, chunk, and embed the content
- Ask a question in the input box and review the answer and cited sources

Note: The FAISS index is saved as `faiss_store_openai.pkl`. Delete this file to force a rebuild.

---

## Configuration
- **Model temperature and tokens**: Edit `OpenAI(temperature=0.9, max_tokens=500)` in `main.py` to suit your needs.
- **Chunking behavior**: Adjust `chunk_size` and `separators` in the `RecursiveCharacterTextSplitter` for different content lengths.
- **Number of URLs**: Change the loop that renders the three URL inputs in the sidebar.

---

## How It Works
1) **Load**: `UnstructuredURLLoader` fetches page content from the provided URLs.
2) **Split**: `RecursiveCharacterTextSplitter` chunks text for better embedding coverage (default 1,000 tokens).
3) **Embed**: `OpenAIEmbeddings` converts chunks to vectors.
4) **Index**: `FAISS` stores vectors locally and is serialized to `faiss_store_openai.pkl`.
5) **Retrieve + Generate**: `RetrievalQAWithSourcesChain` uses the index to answer queries and cite sources using an OpenAI LLM.

---

## Troubleshooting
- **OpenAI errors**: Ensure `OPENAI_API_KEY` is set and you have API credits or access.
- **Windows "libmagic"/file type errors**: This project includes `python-magic-bin` to avoid `libmagic` issues on Windows. If you still see errors, reinstall `python-magic-bin` and restart your shell.
- **FAISS issues**: If the FAISS index becomes corrupted, delete `faiss_store_openai.pkl` and reprocess URLs.
- **Rate limits**: Reduce the number/length of URLs, lower `max_tokens`, or retry later.

---

## Project Structure
```
news_research_tool/
├── main.py                 # Streamlit app entry point
├── requirements.txt        # Project dependencies
├── faiss_store_openai.pkl  # FAISS index (generated locally)
└── .env                    # OpenAI API key (user-provided)
```

Built with ❤️ using LangChain, FAISS, and Streamlit.