# NeuroScribe | Supplemental Engineering Documentation

This documentation covers the technical implementation details, deployment strategy, and concrete operational structures that extend beyond the initial product requirements.

---

## 📅 Project Timeline & Milestones

* **Project Commencement:** June 14, 2026
* **Target Completion Date:** August 19, 2026 
* **Operational Window:** This 9-week development runway is strategically timed to wrap up functional end-to-end integration and system testing immediately prior to the start of the MSCS program term.
## 🛠️ Complete Tech Stack

* **Languages:** Python, SQL
* **AI/ML Frameworks:** PyTorch, Torchvision, medSpacy, Hugging Face Transformers
* **Computer Vision & Data Parsing:** OpenCV, pdfplumber
* **Cloud Infrastructure (AWS):** Step Functions, ECS Fargate, SageMaker Inference Endpoints, Lambda, SQS, S3, EventBridge, KMS, VPC

---

## 📂 Project Structure

```text
├── .github/workflows/      # CI/CD automated test and deployment pipelines
├── src/
│   ├── ingestion/          # PDF processing, text/image separation (Phase 1)
│   ├── nlp_engine/         # medSpacy named entity extraction (Phase 2)
│   ├── vision_engine/      # PyTorch CNN anomaly classification (Phase 3)
│   └── data_fusion/        # AWS Lambda for schema validation & output (Phase 4)
├── infrastructure/         # Infrastructure as Code (IaC / Terraform / CDK configuration)
├── tests/                  # Unit and end-to-end integration test suites
├── requirements.txt        # Core Python dependencies
└── README.md
