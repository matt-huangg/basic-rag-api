# Basic RAG API

A simple local Retrieval-Augmented Generation API built with FastAPI, ChromaDB, and Ollama.

This project uses the text in `profile.txt` as a small knowledge base. The `build_knowledge_base.py` script splits that profile into chunks, stores them in ChromaDB, and uses Ollama to create embeddings. The FastAPI app then answers questions by retrieving relevant profile chunks and sending them to a local Ollama chat model.

## What This Project Does

1. Reads your personal profile from `profile.txt`.
2. Splits the profile into paragraph chunks.
3. Stores those chunks in a local ChromaDB database.
4. Accepts a question through the API.
5. Retrieves the most relevant profile chunks.
6. Sends the question and retrieved context to Ollama.
7. Returns the answer and the context used.

## Project Files

```text
basic-rag-api/
  build_knowledge_base.py  # Builds the local ChromaDB knowledge base
  main.py                  # FastAPI app with the /ask endpoint
  profile.txt              # Source text for the knowledge base
  chroma_db/               # Generated local ChromaDB data
  README.md
```

## Requirements

- Python 3
- Ollama
- FastAPI
- Uvicorn
- ChromaDB
- Ollama Python package

## Setup

From inside the project folder:

```bash
cd basic-rag-api
```

Create and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

Install the Python packages:

```bash
python3 -m pip install fastapi uvicorn chromadb ollama
```

Install Ollama from:

```text
https://ollama.com
```

Pull the embedding model:

```bash
ollama pull nomic-embed-text
```

Pull the chat model used by `main.py`:

```bash
ollama pull qwen2.5:0.5b
```

Make sure Ollama is running before building the knowledge base or asking questions.

## Build the Knowledge Base

After editing `profile.txt`, run:

```bash
python build_knowledge_base.py
```

This creates or updates the local `chroma_db/` folder.

## Run the API

Start the FastAPI server:

```bash
uvicorn main:app --reload
```

The API will run at:

```text
http://127.0.0.1:8000
```

FastAPI docs are available at:

```text
http://127.0.0.1:8000/docs
```

## Ask a Question

Use the `/ask` endpoint with a `question` query parameter:

```bash
curl "http://127.0.0.1:8000/ask?question=What%20am%20I%20learning%20about%3F"
```

Example response:

```json
{
  "question": "What am I learning about?",
  "answer": "You are currently learning about cloud computing, AI, and DevOps.",
  "context_used": [
    "I'm currently learning about cloud computing, AI, and DevOps."
  ]
}
```

## Notes

- Run commands from the `basic-rag-api` folder so the scripts can find `profile.txt` and `chroma_db/`.
- If you change `profile.txt`, run `python build_knowledge_base.py` again.
- The API uses `nomic-embed-text` for embeddings and `qwen2.5:0.5b` for answering questions.
