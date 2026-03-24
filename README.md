# Chatbot Conversation Memory — Cosmos DB + FastAPI

> A RAG chat backend focused on **conversation memory**: multi-turn history persisted in Azure Cosmos DB, with a warm in-process cache and graceful degraded mode when storage is unavailable.

![python](https://img.shields.io/badge/python-3.11-blue) ![cloud](https://img.shields.io/badge/Azure-commercial-0078D4) ![license](https://img.shields.io/badge/license-MIT-lightgrey)

## What this project is
The piece most RAG demos skip: **durable, multi-user conversation state**. It shows how to persist chat history, restore it on cold start, and keep serving when the store is down — the memory layer behind a real assistant.

## What it actually does (implemented)
- **Conversation persistence** in **Azure Cosmos DB** (create/list/rename/soft-delete).
- **Warm in-process cache** for active sessions; **cold-start restore** from Cosmos.
- **Degraded mode** — keeps answering (in-memory only) if Cosmos is unavailable.
- **Streaming RAG** (hybrid retrieval + Azure OpenAI) on the Microsoft Agent Framework.
- Per-user isolation; Streamlit front end.

## Architecture
```mermaid
flowchart TD
  UI[Streamlit UI] --> API[FastAPI]
  API --> MEM[Session cache]
  MEM <-->|persist / restore| COSMOS[(Cosmos DB · history)]
  API --> RAG[Retrieval + Azure OpenAI]
```

## Run it
```bash
cp .env.example .env       # Azure OpenAI / AI Search / Cosmos values
pip install -r backend/requirements.txt
uvicorn app.main:app --reload
streamlit run frontend/app.py
```

---
