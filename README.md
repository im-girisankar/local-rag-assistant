# local-rag-assistant

Privacy-first retrieval-augmented generation (RAG) over local files. Runs **100% offline — no API keys, no cloud, no GPU required**.

**Product/PM angle**: enterprises and individuals with sensitive documents (medical records, legal files, private notes) who need intelligent Q&A without sending data to the cloud.

## Features
- Ingest `.txt` and `.md` files with overlapping chunking
- Deterministic hashing embedder (zero deps) or sentence-transformers (optional)
- In-memory cosine-similarity vector store with optional disk persistence
- Cited answers: every answer includes the source chunk IDs used
- Lazy Ollama integration — optional, not required for core functionality

## Status — Milestone Roadmap

| Tag | Milestone | Status |
|-----|-----------|--------|
| m1  | File ingestion + hashing embedder + in-memory vector store | ✅ Done |
| m2  | LLM interface (mock + lazy Ollama) + RAG pipeline with cited answers | ✅ Done |
| m3  | CLI (`lra ingest` / `lra ask`), config, README | ✅ Done |
| m4  | Vector store persistence (save/load) + merge/clear + tests | ✅ Done |

## Quick Start

```bash
pip install -e ".[dev]"        # core + dev deps (no GPU needed)
lra ingest ./my-docs/
lra ask "What is the refund policy?"
```

> **Note:** By default the Q&A backend is a deterministic **mock LLM** that returns
> canned responses — it does **not** perform real language generation. For genuine
> question-answering you must configure Ollama (see *Usage with Ollama* below).

## Usage with Ollama (optional)
```bash
pip install -e ".[llm]"
ollama pull llama3
echo '{"llm_backend": "ollama", "ollama_model": "llama3"}' > .lra/config.json
lra --config .lra/config.json ask "Summarise the documents"
```

## Running Tests
```bash
pytest
ruff check src tests
```
