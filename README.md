# RAG + Agentic AI: Consulting Firm Knowledge Assistant

## What it does
An AI agent that answers questions about a consulting firm's services, pricing, FAQs, and case studies by retrieving relevant information from the company's own documents (RAG), and can hold a multi-turn conversation with memory.

## Architecture
**Ingestion pipeline:**
Google Drive (source documents) → Extract text → Split into chunks → Google Gemini Embeddings → Vector Store

**Query pipeline (Agentic AI):**
Chat message → AI Agent (Gemini chat model + conversation memory) → calls Vector Store as a tool → retrieves relevant document chunks → generates grounded answer

## Tools used
- **n8n** — workflow orchestration (no-code)
- **Google Gemini** — chat model + embeddings (free tier, no billing card required)
- **n8n Simple Vector Store** — in-memory vector database for this demo
- **Google Drive** — source document storage

## Design notes
- This build uses n8n's in-memory Simple Vector Store rather than a persistent vector database (e.g. Pinecone) — a deliberate scope choice for a portfolio demo. In-memory data resets when the n8n session restarts, so the ingestion workflow must be re-run before querying in a new session. For production use, swapping in a persistent vector store (Pinecone, Qdrant, Supabase) is a drop-in change to the Vector Store node.
- The agent includes conversational memory, so follow-up questions ("how much does the first one cost?") resolve correctly using prior context.

## How to import this workflow
1. Open n8n → Workflows → Import from File
2. Select `workflow.json` from this folder
3. Reconnect your own Google Drive, Google Gemini, and (optionally) Pinecone credentials
4. Run the ingestion workflow first, then use the chat interface to query
