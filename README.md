# Local RAG

Fully local RAG with **LlamaIndex** + **Ollama**. No API keys — embeddings and inference both run on your machine. Every answer ships with its retrieved sources and similarity scores.

## Setup

```bash
ollama pull qwen3:30b-a3b
ollama pull nomic-embed-text

pip install llama-index llama-index-llms-ollama llama-index-embeddings-ollama
```

Drop your documents in `data/` (pdf, txt, md, docx — subfolders fine).

## Usage

```bash
python rag.py
```

```
> what is the main argument of the paper

The paper argues that ...

--- Sources ---
[0.812] paper.pdf: The central claim advanced here is that...

> save experiment_01
Saved to results/experiment_01.txt
```

First run embeds the corpus into `storage/`; after that it loads in seconds.

## Config

Top of `rag.py` — model tag, `chunk_size`, `similarity_top_k`, `temperature`.

**Gotchas:**

- Ollama defaults `num_ctx` to 4096 and silently truncates retrieved chunks. `context_window=32768` overrides it.
- `thinking=False` suppresses qwen3's reasoning trace. On older `llama-index-llms-ollama`, use `additional_kwargs={"think": False}`.
- `storage/` is tied to one config. Delete it after changing the corpus, embedder, or chunk size — otherwise the stale index loads without warning.

## Troubleshooting

| Symptom | Fix |
|---|---|
| `Connection refused` | Ollama not running — `ollama serve` |
| `model "..." not found` | Copy the tag verbatim from `ollama list` |
| Answers ignore your docs | Delete `storage/` and rebuild |

MIT
