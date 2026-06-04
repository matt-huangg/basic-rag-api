# basic-rag-api

A basic Retrieval-Augmented Generation API built with FastAPI, Ollama, and ChromaDB.

The goal of this project is to expose a simple local RAG workflow over HTTP:

1. Ingest documents.
2. Chunk and embed the content.
3. Store embeddings in ChromaDB.
4. Retrieve relevant context for a user question.
5. Generate an answer with a local Ollama model.

## Tech Stack

- FastAPI for the HTTP API
- Ollama for local LLM and embedding model inference
- ChromaDB for local vector storage
- Python for the application runtime

## Planned API

### Health Check

```http
GET /health
```

Returns a basic service status.

### Ingest Documents

```http
POST /documents
```

Adds text content to the vector database.

Example request:

```json
{
  "text": "FastAPI is a modern Python web framework.",
  "metadata": {
    "source": "manual-note"
  }
}
```

### Ask a Question

```http
POST /query
```

Retrieves relevant chunks from ChromaDB and sends them to Ollama with the user's question.

Example request:

```json
{
  "question": "What is FastAPI?"
}
```

Example response:

```json
{
  "answer": "FastAPI is a modern Python web framework...",
  "sources": []
}
```

## Local Development

### 1. Install Ollama

Install Ollama from:

```text
https://ollama.com
```

Then pull a chat model:

```bash
ollama pull llama3.1
```

Optionally pull an embedding model:

```bash
ollama pull nomic-embed-text
```

### 2. Create a Python Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

This project will use dependencies similar to:

```bash
pip install fastapi uvicorn chromadb ollama pydantic
```

Once a `requirements.txt` file exists, install from it instead:

```bash
pip install -r requirements.txt
```

### 4. Run the API

```bash
uvicorn app.main:app --reload
```

The API should be available at:

```text
http://127.0.0.1:8000
```

FastAPI docs should be available at:

```text
http://127.0.0.1:8000/docs
```

## Project Structure

Planned structure:

```text
basic-rag-api/
  app/
    main.py
    api/
    core/
    services/
    models/
  data/
  tests/
  README.md
  requirements.txt
```

## Notes

- Ollama must be running locally before the API can generate answers.
- ChromaDB can run embedded locally for the first version.
- The initial version should prioritize a small working RAG loop before adding auth, background jobs, or file upload support.
