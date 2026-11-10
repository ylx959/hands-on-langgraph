# Hands-on LangGraph

## Introduction

A hands-on collection of [LangGraph](https://langchain-ai.github.io/langgraph/) examples, going from simple deterministic workflows to a full agentic chatbot. The notebooks are numbered in learning order.

All examples use **Google Gemini** (`gemini-2.5-flash`) through `langchain-google-genai`.

## Highlights

- **Workflow patterns**: sequential, prompt chaining, parallel, conditional routing, and iterative (evaluator–optimizer) loops
- **State and memory**: persistence with checkpointers, threads, and SQLite storage
- **Control**: human-in-the-loop (HITL) with `interrupt`, and subgraphs
- **Production topics**: guardrails, AI agent evaluation, and an LLM gateway with LiteLLM
- **Agentic Chatbot**: a Streamlit chatbot that adds streaming, threads, tools, RAG, and HITL step by step

## How to Start

### 1. Prerequisites

- [uv](https://docs.astral.sh/uv/) (`brew install uv`)
- Python 3.11+ (uv will download it automatically if missing)

### 2. Install dependencies

```bash
uv sync
```

This creates `.venv/` and installs everything from `uv.lock`.

### 3. Set up API keys

Create a `.env` file in the project root. The `Agentic-Chatbot-using-LangGraph/` and `Agentic-Chatbot-using-LangGraph/notebooks/` folders each need one too.

```env
GOOGLE_API_KEY=your-gemini-key        # required: https://aistudio.google.com/apikey
TAVILY_API_KEY=your-tavily-key        # web search tool (chatbot tool / RAG / HITL)
GROQ_API_KEY=your-groq-key            # optional: LLM gateway demo
OPENAI_API_KEY=your-openai-key        # optional: LLM gateway demo
OPENWEATHER_API_KEY=your-key          # optional: weather tool

# optional: LangSmith tracing
LANGSMITH_TRACING=false
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_API_KEY=your-langsmith-key
LANGSMITH_PROJECT=hands-on-langgraph
```

### 4. Run the notebooks

```bash
uv run jupyter lab
```

In VS Code, open any `.ipynb` file and select the `.venv` Python kernel.

### 5. Run the Streamlit chatbot

```bash
cd Agentic-Chatbot-using-LangGraph
uv run streamlit run app_simple.py     # or app_thread.py / app_db.py / app_tool.py / app_rag.py / app_hitl.py
```

Run it from inside that folder so `.env`, `chatbot.db`, and `faiss_db/` resolve correctly.

### Managing packages

```bash
uv add <package>        # add a dependency
uv remove <package>     # remove a dependency
uv sync                 # sync the environment with uv.lock
```

## Project Structure

Here's how the repository is organized:

```
hands-on-langgraph/
├── pyproject.toml                          # project metadata and dependencies (uv)
├── uv.lock                                 # locked dependency versions
├── .env                                    # API keys (not committed)
│
├── 1_Temperature_Conversion_workflow.ipynb # basic StateGraph, no LLM
├── 2_Simple_QA_LLM_Workflow.ipynb          # first LLM node
├── 3_Prompt_Chaining_Workflow.ipynb        # sequential LLM calls
├── 4_Employee_analytics_Workflow.ipynb     # parallel workflow (fan-out / fan-in)
├── 5_Essay_workflow.ipynb                  # parallel evaluation + structured output
├── 6_Content_Moderation_Workflow.ipynb     # conditional routing
├── 7_Review_workflow.ipynb                 # conditional workflow with sentiment
├── 8_Iterative_Workflows.ipynb             # generator / evaluator / optimizer loop
├── 9_Persistence.ipynb                     # checkpointers, threads, time travel
├── 10_HITL.ipynb                           # human-in-the-loop with interrupt
├── 11_subgraphs.ipynb                      # subgraphs with separate state
├── 12_subgraph_shared.ipynb                # subgraphs with shared state
├── 13_guardrails_crash_course.ipynb        # guardrails and agent middleware (PII, HITL)
├── AI_Agent_Evaluation.ipynb               # evaluators, datasets, LLM-as-a-judge
├── llm_gateway.ipynb                       # multi-provider routing and fallbacks with LiteLLM
│
├── *.excalidraw                            # diagrams (HITL, Persistence, Subgraphs, demo)
├── images/                                 # images used in notebooks
│
└── Agentic-Chatbot-using-LangGraph/
    ├── agentic_chatbot_backend.py          # basic chatbot graph (in-memory)
    ├── agentic_chatbot_db_backend.py       # + SQLite persistence
    ├── agentic_chatbot_tool_backend.py     # + tools (Tavily search, etc.)
    ├── agentic_chatbot_rag_backend.py      # + RAG over a PDF (FAISS)
    ├── agentic_chatbot_hitl_backend.py     # + human-in-the-loop approval
    ├── app_simple.py                       # Streamlit UI: streaming chat
    ├── app_thread.py                       # Streamlit UI: multiple conversation threads
    ├── app_db.py                           # Streamlit UI: persistent threads
    ├── app_tool.py                         # Streamlit UI: tool calling
    ├── app_rag.py                          # Streamlit UI: RAG
    ├── app_hitl.py                         # Streamlit UI: HITL
    ├── chatbot_with_hitl.py                # standalone HITL chatbot script
    ├── chatbot_without_hitl.py             # standalone chatbot script without HITL
    ├── non_stream.py / test.py             # non-streaming UI and quick test
    ├── chatbot.db                          # SQLite checkpoint store
    ├── faiss_db/                           # FAISS vector index
    └── notebooks/                          # step-by-step demos (chatbot, tools, RAG, HITL)
```
