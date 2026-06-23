# local-rag-assistant — Working Notes

## Architecture
- `ingestion.py` — file reading + chunking (no deps)
- `embedder.py` — HashEmbedder (default, no deps) + lazy SentenceTransformerEmbedder
- `vector_store.py` — in-memory cosine-similarity store + pickle persistence + merge/clear
- `llm.py` — MockLLM (testing) + lazy OllamaLLM
- `pipeline.py` — wires embedder + store + llm into RAG
- `cli.py` — argparse CLI; loads store from disk if present
- `config.py` — Config dataclass with JSON loading

## Design Decisions
- Heavy deps (sentence-transformers, ollama) lazy-imported inside functions
- HashEmbedder ensures all tests pass with zero external deps
- Vector store save/load uses pickle for simplicity (M4)
- VectorStore.merge() combines two stores; VectorStore.clear() resets state
- All tests use MockLLM and HashEmbedder — no network, no GPU

## Dev Commands
```bash
pip install -e ".[dev]"
pytest
ruff check src tests
lra ingest README.md
lra ask "What is this project about?"
```
