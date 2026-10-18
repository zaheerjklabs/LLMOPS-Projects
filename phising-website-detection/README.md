# 🛡️ PhishGuard: Phishing Website Detection MLOps Pipeline

End-to-End MLOps pipeline for real-time cyber-threat detection and phishing URL classification using **XGBoost**, **Neural Networks (ANN)**, **MLflow**, and **FastAPI**.

---

## 🎯 Overview

Phishing attacks and malicious domains evolve rapidly, making static blocklists insufficient. **PhishGuard** delivers:
* **Real-Time Feature Engineering:** Dynamically extracts 30+ features across URL structure, domain registration age, DNS resolution records, SSL certificates, and HTML DOM elements (`src/website_feature_extraction.py`).
* **Dual-Model Architecture:** Trains and evaluates both gradient boosted decision trees (**XGBoost**) and Artificial Neural Networks (**MLP / ANN**).
* **MLflow Experiment Tracking & Registry:** Logs parameters, metrics (ROC-AUC, Precision, Recall, F1), confusion matrices, and serialized model artifacts.
* **Production Serving & CI/CD:** Containerized FastAPI REST API (`api/main.py`) with Docker packaging and GitHub Actions automated deployment.

---

## 🏗️ Architecture

```
Raw URL Input (via API or Web Form)
       │
       ▼
Feature Extraction Engine (`src/website_feature_extraction.py`)
 ├── Lexical URL Parsing (Length, subdomains, special chars, IP detection)
 ├── Domain & DNS Checks (WHOIS registration, MX records, TTL)
 └── SSL & Content Inspection (HTTPS validation, HTML iframe/anchor checks)
       │
       ▼ [30 Feature Vector]
Pre-trained Preprocessor & Scaler (`artifacts/scaler.pkl`)
       │
       ▼
Ensemble Model Inference
 ├── Model A: XGBoost Classifier (`artifacts/xgb_model.pkl`)
 └── Model B: Neural Network MLP (`artifacts/ann_mlp_model.pkl`)
       │
       ▼
Prediction Output: [Legitimate (0) | Phishing (1)] + Confidence Score
```

---

## 🚀 Quickstart

### 1. Environment Setup
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

### 2. Run Training Pipeline & MLflow Tracking
```bash
# Execute end-to-end data ingestion, preprocessing, training & artifact generation
python run_pipeline.py

# Launch MLflow UI to inspect runs, metrics, and models
mlflow ui --backend-store-uri sqlite:///mlflow.db
```

### 3. Launch FastAPI Prediction Service
```bash
uvicorn api.main:app --reload --host 0.0.0.0 --port 8000
```
Open `http://localhost:8000` to access the prediction portal and test suspicious URLs in real-time.

---

## 🐳 Docker Containerization

```bash
# Build Docker image
docker build -t phishguard-ai:latest .

# Run container
docker run -p 8000:8000 phishguard-ai:latest
```
Automated deployment workflows are defined in `.github/workflows/deploy.yaml`.
