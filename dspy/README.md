# DSPy / MCP / MLflow demos

Course demos for LLM-SE 2027. Separate uv project from the repo root, on
Python 3.14.

```bash
uv sync
```

## What runs standalone

```bash
uv run python discover_mcp_tools.py   # lists the airline-agent MCP tools
uv run python radon.py                # matplotlib illustration of Radon's theorem
```

## What needs a model

`hello.py`, `intro-01.py` and the notebooks point at a local **Ollama** server
(`http://localhost:11434`) — start it and pull the model the file names
(`devstral`, `llama3.2`) before running them.

`mlflow_demo1.py` needs both a tracking server (`mlflow server` on port 5000)
and an xAI key:

```bash
export XAI_API_KEY=...      # never hardcode it; see ../.env.example
uv run python mlflow_demo1.py
```

## Pinned dependencies

- **`mcp<2`** — the demos use `mcp.server.fastmcp.FastMCP`, which mcp 2.x
  renamed to `MCPServer`. Bump only alongside a code migration.
- **`dspy`** (not `dspy-ai`) — `dspy-ai` is now an empty shim that re-exports
  `dspy`, so pinning it does not constrain the real version.

## Large files (not in git)

The retrieval notebooks use a Wikipedia abstracts corpus and a BM25 index
(~5 GB): `wiki.abstracts.2017.jsonl`, `wiki.abstracts.2017.tar.gz` and
`wiki.abstracts.2017.bm25/`. These are gitignored — download or rebuild them
locally.
