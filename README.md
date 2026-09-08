# Software Engineering with AI — LLM-SE 2027

Course materials (syllabus, lecture slides, and code demos) for *Software
Engineering with AI*, Ben-Gurion University.

## Contents

| Path | What's in it |
| --- | --- |
| [`syllabus-software-engineering-with-ai.md`](syllabus-software-engineering-with-ai.md) | Current syllabus |
| [`Intro/`](Intro/) | Introductory slides: course project, PRDs, GitHub Spec-Kit |
| [`LLMs/`](LLMs/) | LLM fundamentals: tokenization, transformers, decoding, pre/post-training, prompting |
| [`dspy/`](dspy/) | DSPy demos, MCP client/server examples, MLflow tracing (separate uv project, Python 3.11) |

## Setup

This repo uses [uv](https://docs.astral.sh/uv/) with Python 3.14 (pinned in
`.python-version`).

```bash
uv sync            # creates .venv and installs the dev group
uv run jupyter lab # launch notebooks
```

The `dspy/` demos are a **separate uv project** pinned to Python 3.11 (some of
its dependencies do not yet build on 3.14). Work on them from that directory:

```bash
cd dspy
uv sync
uv run python hello.py
```

## Large files

The DSPy retrieval demos use a Wikipedia abstracts corpus and its BM25 index
(~5 GB). These are **not** in git — see `.gitignore`. Regenerate or re-download
them locally as described in [`dspy/README.md`](dspy/README.md).
