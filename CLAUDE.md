# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Jupyter notebook-based educational repository accompanying a [video playlist](https://youtube.com/playlist?list=PLfaIDFEXuae2LXbO1_PKyVJiQ23ZztA0x) that teaches Retrieval Augmented Generation (RAG) from scratch using LangChain, LangGraph, and OpenAI.

## Running Notebooks

Launch Jupyter and run notebooks interactively:
```bash
jupyter notebook
# or
jupyter lab
```

Install dependencies (each notebook includes its own install cell):
```bash
pip install langchain_community tiktoken langchain-openai langchainhub chromadb langchain
```

Some notebooks require additional packages:
```bash
pip install youtube-transcript-api pytube cohere ragatouille
```

## Environment Setup

Each notebook requires these environment variables set at the top:
- `OPENAI_API_KEY` — required for all notebooks
- `LANGCHAIN_API_KEY` — for LangSmith tracing (optional but used throughout)
- `LANGCHAIN_TRACING_V2=true` and `LANGCHAIN_ENDPOINT` — for LangSmith
- `COHERE_API_KEY` — required for notebooks 15+ that use Cohere re-ranking

## Notebook Structure

The notebooks are numbered sequentially and build on each other conceptually:

| Notebook | Parts | Topics |
|----------|-------|--------|
| `rag_from_scratch_1_to_4.ipynb` | 1–4 | Indexing, retrieval, generation basics; embeddings, vectorstores (Chroma), RAG chains |
| `rag_from_scratch_5_to_9.ipynb` | 5–9 | Query transformations: Multi-Query, RAG-Fusion (RRF), Decomposition, Step-Back, HyDE |
| `rag_from_scratch_10_and_11.ipynb` | 10–11 | Routing (logical via structured output, semantic via embeddings); query construction with metadata filters |
| `rag_from_scratch_12_to_14.ipynb` | 12–14 | Advanced indexing: Multi-representation (summaries + MultiVectorRetriever), RAPTOR, ColBERT via RAGatouille |
| `rag_from_scratch_15_to_18.ipynb` | 15–18 | Advanced retrieval: Re-ranking (RRF + Cohere), CRAG (LangGraph), Self-RAG (LangGraph), long context impact |

## Architecture Patterns

All notebooks follow the same three-stage RAG pipeline:
1. **Indexing**: Load (`WebBaseLoader`/`YoutubeLoader`) → Split (`RecursiveCharacterTextSplitter`) → Embed (`OpenAIEmbeddings`) → Store (`Chroma`)
2. **Retrieval**: Query → Vectorstore retriever → (optional) re-ranking/fusion
3. **Generation**: Retrieved docs + question → Prompt template → LLM (`ChatOpenAI`) → `StrOutputParser`

Chains are composed using LangChain Expression Language (LCEL) with the `|` pipe operator. The primary document used across most notebooks is [Lilian Weng's LLM agents blog post](https://lilianweng.github.io/posts/2023-06-23-agent/).

## Editing Notebooks

Use the `NotebookEdit` tool (not raw file edits) when modifying `.ipynb` files, as notebooks are JSON with cell IDs that must be preserved.
