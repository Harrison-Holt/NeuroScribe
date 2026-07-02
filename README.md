# NeuroScribe | Supplemental Engineering Documentation

This documentation outlines the architectural blueprints, system design patterns, and cloud deployment strategies for NeuroScribe, a decoupled, multi-modal clinical intelligence pipeline.

---

## 📅 Project Phase & Strategic Timeline
* **Project Commencement:** June 14, 2026
* **Current Phase:** **System Architecture & Cloud Design Verification** (Active project development paused until the Clemson MSCS program commences)
* **Target Completion Date:** May 2028 (Scheduled for final production deployment to coincide with MSCS graduation)

---


## 🛠️ Complete Tech Stack & Cloud Infrastructure

### Core Processing Languages
* **Infrastructure Orchestration:** Python, SQL, HCL (Terraform)

### AI/ML & Computer Vision Blueprints
* **Natural Language Processing:** medSpacy, Hugging Face Transformers
* **Computer Vision & Parsing:** OpenCV, pdfplumber
* **Deep Learning Runtime:** PyTorch, Torchvision

### Enterprise Cloud Architecture (AWS)
* **Orchestration & Compute:** AWS Step Functions, AWS ECS Fargate, Lambda
* **Model Serving:** Amazon SageMaker Inference Endpoints (vLLM warm-server configurations)
* **Ingestion & Messaging:** Amazon S3, Amazon SQS, Amazon EventBridge
* **Security & Networking:** AWS KMS (Envelope Encryption), Private VPC (Isolated Subnets)

---

## 🏗️ System Architecture & Data Flow

```text
[Patient PDF Ingestion] 
       │
       ▼
 ┌───────────┐      EventBridge      ┌──────────────────────┐
 │  S3 Land  │ ────────────────────► │  AWS Step Functions  │
 └───────────┘                       └──────────────────────┘
                                                 │
                        ┌────────────────────────┴────────────────────────┐
                        ▼                                                 ▼
            ┌───────────────────────┐                         ┌───────────────────────┐
            │   Phase 1: Ingestion  │                         │   Phase 2: NLP Engine │
            │   (pdfplumber / S3)   │                         │    (medSpacy / ECS)   │
            └───────────────────────┘                         └───────────────────────┘
                        │                                                 │
                        └────────────────────────┬────────────────────────┘
                                                 ▼
                                     ┌───────────────────────┐
                                     │ Phase 3: Vision Node  │
                                     │ (PyTorch / SageMaker) │
                                     └───────────────────────┘
                                                 │
                                                 ▼
                                     ┌───────────────────────┐
                                     │ Phase 4: Data Fusion  │
                                     │ (JSON Schema / S3)    │
                                     └───────────────────────┘
```

1. **Ingestion Loop:** Unstructured medical intake PDFs land in an encrypted Amazon S3 bucket. An EventBridge rule triggers an AWS Step Functions state machine.
2. **Text & Image Dissection:** An ECS Fargate task running `pdfplumber` isolates unstructured text strings while utilizing `OpenCV` to extract, crop, and isolate embedded patient MRI slices.
3. **Clinical Entity Extraction:** Isolated text strings are passed to an asynchronous container running `medSpacy` to extract clinical entities, flags, and anatomical notes.
4. **Anomalous Vision Tracking:** Isolated MRI image segments are batched and routed to an Amazon SageMaker Inference Endpoint hosting a custom PyTorch CNN to flag anatomical anomalies.
5. **Data Fusion & Schema Validation:** An AWS Lambda function coordinates the outputs from the NLP and Vision nodes, fuses them into a structured, HIPAA-compliant JSON payload, validates it against a strict relational schema, and writes the output back to a secure delivery S3 bucket.

---

## 📂 Project Directory Scaffold

```text
├── .github/workflows/       # CI/CD automated cloud linting and deployment pipelines
├── src/
│   ├── ingestion/           # Data isolation: PDF processing, text/image separation (Phase 1)
│   ├── nlp_engine/          # Semantic analysis: medSpacy named entity extraction (Phase 2)
│   ├── vision_engine/       # Visual computing: PyTorch CNN anomaly classification (Phase 3)
│   └── data_fusion/         # Output orchestration: AWS Lambda schema validation & generation (Phase 4)
├── infrastructure/          # Infrastructure as Code (IaC / Terraform configuration)
│   ├── main.tf              # Base VPC, S3 buckets, and KMS encryption configurations
│   ├── step_functions.json  # State Machine declarative JSON definition
│   └── variables.tf         # Environment-level configurations
├── templates/               # Standardized HIPAA-ready JSON target schemas
└── README.md                # System Overview & Developer Quickstart
```
