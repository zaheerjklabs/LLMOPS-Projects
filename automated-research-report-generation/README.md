# 🔬 Automated Research Report Generation System

Autonomous multi-agent research, interview, and report synthesis platform built with **LangGraph**, **LangChain**, and **FastAPI**.

---

## 🎯 Overview

Synthesizing deep, well-referenced research reports manually is slow and repetitive. This platform automates the end-to-end research lifecycle:
1. **Interactive Interviewing**: Clarifies research scope, user background, depth, and output constraints.
2. **Autonomous Multi-Source Search**: Gathers real-time web intelligence using Tavily and Wikipedia APIs.
3. **Drafting & Synthesis**: Employs dynamic multi-LLM orchestration (DeepSeek-R1 via Groq, Gemini-2.0-Flash via Google GenAI, and GPT-4o via OpenAI) to write structured, citation-backed analytical reports.
4. **Publishing**: Compiles formatted Microsoft Word (`.docx`) and PDF (`.pdf`) deliverables.

---

## 🏗️ Architecture

```
User Input / Web UI
       │
       ▼
FastAPI Backend (Session & Auth Management)
       │
       ▼
LangGraph Workflows
 ├── 1. Interview Workflow (Scope definition & persona adaptation)
 └── 2. Report Generation Workflow (Iterative research, query expansion & section synthesis)
       │
 ├── Search Tools (Tavily Search API + Wikipedia API)
 └── LLM Router (DeepSeek-R1 / Gemini-2.0-Flash / GPT-4o)
       │
       ▼
Document Generator (DOCX & PDF with Citations)
```

---

## 🚀 Quickstart

### 1. Environment Setup
```bash
# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configure Environment Variables
Copy `.env.copy` to `.env` and provide your API keys:
```bash
cp .env.copy .env
```
Key configuration parameters:
* `GROQ_API_KEY`: Groq API access for DeepSeek models
* `GOOGLE_API_KEY`: Google Gemini API access
* `OPENAI_API_KEY`: OpenAI GPT-4o access
* `TAVILY_API_KEY`: Tavily Search API key

### 3. Run Application
```bash
uvicorn research_and_analyst.api.main:app --reload --host 0.0.0.0 --port 8000
```
Open your browser at `http://localhost:8000` to access the interactive dashboard.

---

## 🐳 Docker & CI/CD Deployment

```bash
# Build Docker container
docker build -t automated-research-report:latest .

# Run container
docker run -p 8000:8000 --env-file .env automated-research-report:latest
```

The repository includes a production `Jenkinsfile` and Azure setup script (`azure-deploy-jenkins.sh`) for automated build, test, and container registry publishing.