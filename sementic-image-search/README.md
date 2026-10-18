# 🖼️ Multimodal Semantic Image Search Engine

Zero-Shot Visual Discovery and Multimodal Image Search platform powered by **OpenCLIP (ViT-B-32)**, **Qdrant Vector Database**, **FastAPI**, and **Streamlit**.

---

## 🎯 Overview

Keyword-only image search fails when queries contain abstract concepts, stylistic descriptions, or multilingual expressions. This engine provides:
* **Zero-Shot Multimodal Retrieval:** Projects both images and text queries into a shared 512-dimensional vector space using OpenCLIP (`ViT-B-32` on `laion2b_s34b_b79k`).
* **High-Performance Vector Storage:** Integrated with **Qdrant Vector Database** for sub-millisecond approximate nearest neighbor (ANN) similarity search.
* **Intelligent Query Translation:** An LLM translation and query expansion layer (`query_translator.py`) enabling multilingual queries and conceptual refinement.
* **Offline Evaluation Suite:** Automated Jupyter notebooks measuring `Recall@K` and benchmarking embeddings across categories (animals, weapons, general objects).
* **Full-Stack Application:** Responsive Streamlit user interface (`ui/app.py`) backed by an asynchronous FastAPI service (`backend/main.py`).

---

## 🏗️ Architecture

```
User Query (Text or Reference Image)
       │
       ▼
Query Translation & Expansion Layer (`backend/query_translator.py`)
       │
       ▼
OpenCLIP Embedding Engine (`backend/embeddings.py`)
 ├── Text Encoder: 512-dim Normalized Dense Vector
 └── Image Encoder: 512-dim Normalized Dense Vector
       │
       ▼
Qdrant Vector Database (`backend/qdrant_client.py`)
 ├── Cosine Distance Search over Indexed Image Embeddings
 └── Payload Filtering (Categories, tags, metadata)
       │
       ▼
Ranked Result Retrieval (`backend/retriever.py`)
       │
       ▼
Streamlit Visual Discovery Explorer (`ui/app.py`)
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
QDRANT_URL="http://localhost:6333" # or Qdrant Cloud URL
QDRANT_API_KEY="your-qdrant-api-key"
DEVICE="cpu" # or "cuda"
CLIP_MODEL_NAME="ViT-B-32"
CLIP_CHECKPOINT="laion2b_s34b_b79k"
```

### 3. Ingest Image Catalog
```bash
python -m semantic_image_search.backend.ingestion
```

### 4. Launch Backend API & Frontend UI
```bash
# Terminal 1: Launch FastAPI Backend
uvicorn semantic_image_search.backend.main:app --reload --port 8000

# Terminal 2: Launch Streamlit Web UI
streamlit run semantic_image_search/ui/app.py --server.port 8501
```
Open `http://localhost:8501` to start searching images with natural language queries.

### 5. Benchmark & Evaluation
Open `semantic_image_search/notebooks/evaluate_recall_at_k.ipynb` to evaluate retrieval precision and calculate `Recall@1`, `Recall@5`, and `Recall@10` metrics.
