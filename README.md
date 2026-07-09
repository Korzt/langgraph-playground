# 25AI-LangGraph-Playground

A minimal LangGraph + LangChain playground for exploring tool-enabled agents, RAG, and MCP server integrations using Google Gemini and custom Python tools.

## Repository Overview

This repository demonstrates several AI agent patterns built with:

- `langgraph` for agent orchestration and state graphs
- `langchain` and `langchain_google_genai` for LLM calls
- `langchain_community` document loaders and tools
- `langchain_chroma` for vector store retrieval
- `langchain_mcp_adapters` for multi-server MCP tool integration

## Project Structure

- `agents/`
  - `Drafter.py` — document editing assistant with save/load/list tools
  - `ReAct.py` — simple math tool agent using LangGraph tools
  - `ReAct_MCP.py` — agent that loads tools from external MCP servers
  - `RAG_Agent.py` — retrieval-augmented agent using a PDF vector store
- `mcp-servers/`
  - `math-server.py` — example tool server for arithmetic
  - `weather-server.py` — example weather tool server
  - `go-json-server.py` — example product catalog server
- `resources/` — local storage for documents, PDFs, and saved output
- `servers/` — sample Go JSON server and supporting utilities
- `requirements.txt` — Python dependencies
- `Dockerfile` — container image build definition
- `.env` — environment variables (not checked in with secrets)

## Prerequisites

- Python 3.11
- `pip`
- A Google API key set as `GOOGLE_API_KEY` in `.env`
- Optional: Docker if you want to build the container

## Setup

1. Create and activate a Python virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Install dependencies:

```powershell
pip install --upgrade pip
pip install -r requirements.txt
```

3. Copy `.env` and set your Google API key:

```powershell
copy .env.example .env
# then edit .env and set GOOGLE_API_KEY=
```

> If there is no `.env.example`, create `.env` with:
>
> `GOOGLE_API_KEY=your_api_key_here`

## Running the Agents

### 1. Drafter Agent

A writing assistant that can update, save, load, and list files under `resources/`.

```powershell
python agents\Drafter.py
```

### 2. ReAct Example Agent

A small tool-enabled agent that supports arithmetic operations.

```powershell
python agents\ReAct.py
```

### 3. ReAct MCP Agent

Uses the MCP client to load external tool servers and execute tool-enabled prompts.

```powershell
python agents\ReAct_MCP.py
```

### 4. RAG Agent

Loads a local PDF from `resources/` and builds a Chroma vector store for retrieval-augmented generation.

```powershell
python agents\RAG_Agent.py
```

## MCP Servers

The repo includes example MCP tool servers in `mcp-servers/`.

- `math-server.py` — simple math operations
- `weather-server.py` — weather lookup stub
- `go-json-server.py` — product catalog service

These are launched automatically by `agents/ReAct_MCP.py`.

## Docker

Build the container image from the repository root:

```powershell
docker build -t 25ai-langgraph-playground .
```

The current `Dockerfile` installs Python 3.11, Rust toolchain dependencies, and the required Python packages.

## Notes

- The repository is intended as a playground for learning how to wire together LangGraph agents, tools, RAG workflows, and MCP adapters.
- `resources/` is the place to store local PDFs, documents, or other artifacts used by the agents.
- The current sample code uses Gemini models via `ChatGoogleGenerativeAI`.

## License

This project is licensed under the terms in `LICENSE`.
