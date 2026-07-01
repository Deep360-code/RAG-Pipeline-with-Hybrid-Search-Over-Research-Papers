# Agentic Retrieval-Augmented Generation (RAG) Document Curator

A production-grade, asynchronous RAG and multi-agent system engineered to automatically ingest, index, and query academic publications and unstructured research documents. This repository demonstrates a highly scalable AI engineering architecture incorporating hybrid search foundations, distributed task queues, structured observability workflows, and autonomous agent orchestration.

## 🚀 Key Architectural Highlights

* **Autonomous Agentic Orchestration**: Implements self-correcting routing loops via LangGraph. The engine evaluates document relevance, dynamically rewrites ambiguous queries, filters out-of-domain prompts, and executes corrective search procedures.
* **Dual-Engine Hybrid Retrieval**: Merges lexical relevance (OpenSearch BM25) with deep semantic vector embeddings (Jina AI) optimized for high-dimensional academic context retrieval.
* **Production Observability & Tracing**: Full runtime execution tracking, prompt versioning, and latency analysis via localized Langfuse integrations.
* **High-Concurrency Async API**: Built entirely on FastAPI, utilizing Python's `asyncio` to drive low-latency streaming generations and non-blocking service communication.
* **Distributed Task Pipelines**: Decoupled document ingestion and scheduled paper harvesting powered by Apache Airflow workers.

---

## 🏗️ System Topology & Data Flow

```text
                               ┌──────────────────────────┐
                               │  Multi-Interface Layer   │
                               │  (Gradio UI / Telegram)  │
                               └────────────┬─────────────┘
                                            │
                                            ▼
                               ┌──────────────────────────┐
                               │  FastAPI Async Gateway   │
                               └────────────┬─────────────┘
                                            │
                                            ▼
                     ┌──────────────────────────────────────────────┐
                     │ LangGraph Autonomous Agentic Supervisor      │
                     │  - Adaptive Routing  - Out-of-Domain Guard   │
                     └──────┬────────────────────────────┬──────────┘
                            │                            │
                     [Query Rewrite]               [Evaluate Results]
                            │                            │
                            ▼                            ▼
         ┌────────────────────────────────────────────────────────────────┐
         │                  Hybrid Retrieval Ingestion Pipeline           │
         │  ┌───────────────────────┐          ┌───────────────────────┐  │
         │  │ Lexical Index (BM25)  │          │ Dense Vector Engine   │  │
         │  │ (OpenSearch Engine)   │          │ (Jina / Semantic)     │  │
         │  └───────────┬───────────┘          └───────────┬───────────┘  │
         └──────────────┼──────────────────────────────────┼──────────────┘
                        │                                  │
                        └─────────────────┬────────────────┘
                                          ▼
                             ┌──────────────────────────┐
                             │ Cross-Encoder Re-ranker  │
                             └────────────┬─────────────┘
                                          │
                                          ▼
                             ┌──────────────────────────┐
                             │  Ollama / Local LLM      │
                             │  (Streaming Synthesis)   │
                             └──────────────────────────┘
```

---

## 🛠️ Technology Stack

* **Core Runtime:** Python 3.12, UV Package Manager
* **API & UI Layer:** FastAPI, Gradio, Telegram Bot API
* **Search & Vectors:** OpenSearch 2.19, PostgreSQL (Metadata Store), Jina Embeddings
* **Orchestration & Workflow:** LangGraph, Apache Airflow, Redis (Caching Layer)
* **Monitoring:** Langfuse (Open-source LLM Engineering Platform)

---

## 🔧 Getting Started & Deployment

### 📋 System Requirements
* **Docker Engine** with Compose v2.20.0+
* Minimum **8GB RAM** allocated to the Docker daemon
* At least **20GB free disk space** for index persistence

### ⚡ Quick Start

1. **Clone the Repository**
   ```bash
   git clone https://github.com
   cd arxiv-paper-curator
   ```

2. **Environment Configuration**
   Initialize your environment from the baseline configuration template:
   ```bash
   cp .env.example .env
   ```
   *Note: Open the `.env` file and insert your respective external service API credentials (e.g., Jina AI tokens and Langfuse tracking targets) where prompted.*

3. **Install Dependencies Locally**
   Ensure isolated code environment parity across development instances:
   ```bash
   uv sync
   ```

4. **Boot Up the Infrastructure Grid**
   Spin up the full multi-container stack in detached state:
   ```bash
   docker compose up --build -d
   ```

5. **Verify Subsystem Health**
   ```bash
   curl http://localhost:8000/api/v1/health
   ```

---

## 📊 Cluster Endpoint Registry

Once the Docker stack completes initialization, individual services can be analyzed via the following loopback endpoints:

| Infrastructure Service | Local Loopback Address | Purpose |
|:---|:---|:---|
| **FastAPI Core Gateway** | `http://localhost:8000/docs` | Interactive Swagger API Specifications |
| **Gradio Interactive UI** | `http://localhost:7861` | Contextual Research Assistant UI Chat |
| **Langfuse Dashboard** | `http://localhost:3000` | Real-time Prompt Tracing and LLM Metrics |
| **Apache Airflow Orchestrator**| `http://localhost:8080` | Schedule Control for Ingestion DAG Pipelines |
| **OpenSearch Dashboards** | `http://localhost:5601` | Database Health & Vector Search Analysis |

> 🔑 **Credentials Note:** See `airflow/simple_auth_manager_passwords.json.generated` inside your running instances to locate autogenerated secure access passwords for administrative accounts.

---

## 📂 Repository Layout

```text
├── src/                  # Primary Execution Codebase
│   ├── agents/           # LangGraph nodes, semantic grading, and agent logic
│   ├── database/         # Vector index initializers and structural schemas
│   ├── pipeline/         # Deterministic ingestion parser & hybrid indexers
│   └── main.py           # Core FastAPI asynchronous web server routing
├── config/               # Subsystem configurations, search policies, and LLM hyperparameters
├── tests/                # Integrated evaluation suites and unit test layers
├── docker-compose.yml    # Main services orchestration manifest
└── Dockerfile            # Optimized, multi-stage platform container builds
```

## 📄 License

This software utility is open-sourced under the terms of the MIT License. See [LICENSE](LICENSE) for granular clauses.
