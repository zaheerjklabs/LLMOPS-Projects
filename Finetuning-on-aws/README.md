# ⚡ Domain-Specific LLM Fine-Tuning & Serving on AWS

End-to-End Parameter-Efficient Fine-Tuning (PEFT / QLoRA) and Serverless Inference on **AWS SageMaker** and **AWS Lambda**.

---

## 🎯 Overview

Deploying generalized foundation models in specialized domains (such as Pharmaceuticals and Clinical Healthcare) often leads to hallucinations and compliance risks. This project demonstrates:
* **Quantized Low-Rank Adaptation (QLoRA):** 4-bit base model quantization with LoRA adapters for minimal VRAM footprint during causal LM training.
* **AWS SageMaker Distributed Estimator:** Automated provisioning of GPU instances (e.g., `ml.g5.2xlarge`) via SageMaker HuggingFace Estimators.
* **Managed Endpoint Deployment:** Packaging adapters and deploying real-time low-latency SageMaker endpoints.
* **Serverless Bridge:** AWS Lambda integration decoupling frontend clients from direct SageMaker invocation.
* **Clinical Assistant Applications:** Streamlit diagnostic UI and FastAPI service backed by domain knowledge and fine-tuned weights.

---

## 🏗️ Architecture

```
Domain Instruction Data (`pharma_instruction_data.csv`)
       │
       ▼
AWS SageMaker HuggingFace Estimator (`estimator_launcher.ipynb`)
 └── Training Script (`scripts/train.py`)
      ├── 4-Bit Model Quantization (`bitsandbytes`)
      └── LoRA Adapter Weights Training (`peft`)
       │
       ▼
Model Artifacts to Amazon S3
       │
       ▼
SageMaker Real-Time Endpoint (`deployment_of_model.ipynb` + `inference/inference.py`)
       ▲
       │
AWS Lambda Serverless Function (`lambda_function.py`)
       ▲
       │
Interactive Frontend (Streamlit `rag_app_ui.py` / FastAPI `rag_app_backend.py`)
```

---

## 🚀 Workflow Guide

### 1. Training with SageMaker
1. Ensure AWS credentials are configured with SageMaker execution permissions.
2. Launch `estimator_launcher.ipynb` to package `scripts/train.py` and submit the remote SageMaker training job.
3. LoRA adapter weights and checkpoints are automatically saved to S3.

### 2. Deploying Endpoint
1. Open `deployment_of_model.ipynb`.
2. Deploy the HuggingFaceModel container targeting your S3 model artifacts.
3. Configure instance types and autoscaling rules.

### 3. Serverless Invocation via AWS Lambda
Deploy `lambda_function.py` to AWS Lambda:
```python
# The lambda handles payload formatting and invokes SageMaker runtime:
response = sagemaker_runtime.invoke_endpoint(
    EndpointName=ENDPOINT_NAME,
    ContentType="application/json",
    Body=json.dumps(payload)
)
```

### 4. Running the Local Streamlit App
```bash
pip install -r requirements.txt
streamlit run rag_app_ui.py
```
