# 🛍️ E-Commerce Intelligent Product Assistant

Agentic RAG conversational shopping assistant featuring **Model Context Protocol (MCP)**, **DataStax Astra DB**, **Ragas Evaluation**, and **Kubernetes (EKS)** deployment.

---

## 🎯 Overview

Traditional e-commerce chatbots are constrained by static prompts and often hallucinate pricing and inventory. This assistant introduces:
* **Model Context Protocol (MCP) Integration:** LLM connects dynamically to external MCP tool servers (`product_search_server.py`) to query inventory, pricing, and specs.
* **Agentic Multi-Step RAG:** Intelligently routes between localized vector search over product reviews and live external web search fallback.
* **Automated ETL & Scraper:** Interactive scraper pipeline (`data_scrapper.py`, `scrapper_ui.py`) ingesting real-world product catalog data into Astra DB.
* **Continuous LLMOps Evaluation:** Automated benchmark pipeline evaluating **Faithfulness**, **Answer Relevancy**, and **Context Precision** using the **Ragas** framework.

---

## 🏗️ Architecture

```
User Query (Web Chat Interface)
       │
       ▼
FastAPI Application Router (`prod_assistant/router/main.py`)
       │
       ▼
Agentic Workflow Orchestrator
 ├── MCP Client (`prod_assistant/mcp_servers/client.py`)
 │    └── MCP Product Search Server (`product_search_server.py`)
 ├── Astra DB Vector Retriever (`prod_assistant/retriever/retrieval.py`)
 └── Fallback Live Web Search Engine
       │
       ▼
LLM Generation & Response Stream (DeepSeek-R1 / Gemini-2.0 / GPT-4o)
       │
       ▼
Offline / Online Evaluation Gate (`prod_assistant/evaluation/ragas_eval.py`)
```

---

## 🚀 Quickstart

### 1. Environment Setup
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

### 2. Configure Environment Variables
Copy `.env.copy` to `.env` and configure:
```ini
GROQ_API_KEY="your-groq-api-key"
GOOGLE_API_KEY="your-gemini-api-key"
ASTRA_DB_API_ENDPOINT="https://<db-id>-<region>.apps.astra.datastax.com"
ASTRA_DB_APPLICATION_TOKEN="AstraCS:..."
```

### 3. Launching Services
```bash
# Option A: Run Scraper UI to gather product data
streamlit run scrapper_ui.py

# Option B: Run FastAPI Chat Service
python prod_assistant/router/main.py
```
Open `http://localhost:8000` to interact with the shopping assistant.

### 4. Running Ragas Evaluation
```bash
python -m prod_assistant.evaluation.ragas_eval
```

---

## ☸️ Cloud Infrastructure (AWS EKS & Kubernetes)

* **Infrastructure as Code:** `infra/eks-with-ecr.yaml` provisions an AWS EKS cluster with managed node groups and private ECR registry.
* **Kubernetes Manifests:**
  - `k8/deployment.yaml`: Scalable pod replicas, resource limits, and health probes.
  - `k8/service.yaml`: LoadBalancer service routing traffic to application pods.
* **GitHub Actions:** Automated deployment pipelines (`.github/workflows/deploy.yml` and `infra.yml`).