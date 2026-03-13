# Multi-Document RAG Chatbot

A Streamlit-based Retrieval-Augmented Generation (RAG) chatbot that:

- loads multiple PDF files,
- chunks and embeds them with HuggingFace embeddings,
- stores vectors in ChromaDB,
- answers user questions using Groq LLM + conversational memory.

## Project Structure

- `app.py` - Streamlit chat application (retrieval + conversation chain)
- `vectorize_doc.py` - PDF ingestion and vector store creation
- `requirements.txt` - Python dependencies
- `data/` - Put your PDF files here (create this folder if missing)
- `vector_db_dir/` - Chroma persistent vector database (auto-created)
- `config.json` - Local API key config (you create this file)

## Prerequisites

- Python 3.11 or 3.12 recommended
- Windows PowerShell / terminal
- Groq API key

> Note: Python 3.13 may fail for this dependency set because some pinned packages pull NumPy builds that may require local C/C++ build tools.

## Setup

### 1. Clone / open project

Open the workspace and go to this project folder:

```powershell
cd MULTI_DOC_RAG
```

### 2. Create and activate virtual environment

```powershell
py -3.12 -m venv venv
.\venv\Scripts\Activate.ps1
```

If PowerShell blocks activation:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 3. Install dependencies

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 4. Add Groq API key

Create `config.json` in the `MULTI_DOC_RAG` folder:

```json
{
  "GROQ_API_KEY": "your_groq_api_key_here"
}
```

### 5. Add PDF files

Create a `data` folder (if not already present), then add your PDFs:

```powershell
mkdir data
```

### 6. Vectorize documents

```powershell
python vectorize_doc.py
```

Expected output:

```text
Documents Vectorized
```

### 7. Run Streamlit app

```powershell
streamlit run app.py
```

Open the local URL shown by Streamlit (usually http://localhost:8501).

## How It Works

1. `vectorize_doc.py` loads PDFs from `data/`
2. Text is split into chunks
3. Chunks are embedded with `HuggingFaceEmbeddings`
4. Embeddings are stored in Chroma (`vector_db_dir`)
5. `app.py` loads Chroma and creates a `ConversationalRetrievalChain`
6. User chat prompts retrieve relevant chunks and generate grounded answers

## Common Commands

```powershell
# Rebuild vectors after changing PDFs
python vectorize_doc.py

# Launch app
streamlit run app.py
```

## Troubleshooting

### 1. `Fatal error in launcher` when running `pip`

Use:

```powershell
python -m pip install -r requirements.txt
```

If it persists, recreate the virtual environment.

### 2. `No matching distribution found` / NumPy build errors

Use Python 3.11 or 3.12 for this project and recreate `venv`.

### 3. No answers or empty retrieval

- Make sure PDFs exist in `data/`
- Run `python vectorize_doc.py` again
- Confirm `vector_db_dir/` is created

### 4. `ModuleNotFoundError: No module named vectorize_documents`

`app.py` imports `vectorize_documents`, but the file in this project is currently `vectorize_doc.py`.

Use either fix:

- Rename `vectorize_doc.py` to `vectorize_documents.py`, or
- Update the import in `app.py` to match `vectorize_doc`.

## Notes

- Keep `config.json` out of version control.
- Re-run vectorization whenever documents are added/updated.
- If model latency is high, reduce chunk size or the number of source PDFs.

## License

Use this project for learning and experimentation. Add your preferred license if needed.
