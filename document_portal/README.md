# 📄 Enterprise Document Portal

Production-grade Document Intelligence, Multi-Document Comparative Analysis, and Conversational RAG platform built with **FastAPI**, **LangChain**, **FAISS**, and **AWS CloudFormation**.

---

## 🎯 Overview

The **Enterprise Document Portal** provides a secure, scalable document intelligence workflow:
* **Document Analyzer:** Automatically parses long-form business/legal documents, generates structured executive summaries, and extracts key entities.
* **Document Chat (Conversational RAG):** Grounded conversational question-answering with citation and source verification to eliminate hallucinations.
* **Document Comparator:** Side-by-side multi-document cross-comparison highlighting clause differences, amendments, and conflicting terms.

---

## 🏗️ Architecture

```
User Web Client
       │
       ▼
FastAPI Application Layer (`api/main.py`)
       │
  ┌────┴─────────────────────────────┐
  │                                  │
  ▼                                  ▼
Document Ingestion             Vector Store & Retrieval
 ├── PyPDF Loader               ├── FAISS In-Memory / Persistent
 ├── Semantic Text Splitter     └── Google text-embedding-004
  │                                  │
  └──────────────────────────────────┘
       │
       ▼
Intelligence Modules (`src/`)
 ├── `document_analyzer/data_analysis.py`
 ├── `document_chat/retrieval.py`
 └── `document_compare/document_comparator.py`
       │
       ▼
Multi-LLM Router (Groq DeepSeek-R1 / Google Gemini-2.0-Flash)
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
Create a `.env` file in the project root:
```ini
GROQ_API_KEY="your-groq-api-key"
GOOGLE_API_KEY="your-gemini-api-key"
OPENAI_API_KEY="your-openai-api-key"
```

### 3. Run the Application
```bash
python api/main.py
```
Navigate to `http://localhost:8000` to interact with the web interface.

### 4. Run Automated Tests
```bash
pytest tests/
```

---

## ☁️ Cloud Infrastructure & CI/CD (AWS ECS Fargate)

This project includes complete Infrastructure as Code (IaC) and CI/CD pipelines:
* **AWS CloudFormation (`infrastructure/document-portal-cf.yaml`):** Provisions VPC, subnets, ECS cluster, Fargate task definitions, Application Load Balancer, and IAM roles.
* **GitHub Actions Workflows:**
  - `.github/workflows/ci.yaml`: Runs automated linting, test suites, and build checks.
  - `.github/workflows/aws.yaml`: Builds and pushes Docker images to Amazon ECR and updates the Amazon ECS service.
