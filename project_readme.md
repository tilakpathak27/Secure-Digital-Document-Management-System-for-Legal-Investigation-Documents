# Secure Digital Document Management System for Legal & Investigation Documents

![Smart India Hackathon 2026](https://img.shields.io/badge/SIH-2026-blue?style=for-the-badge)
![Problem Statement ID](https://img.shields.io/badge/Problem%20Statement%20ID-SIH26190-brightgreen?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Software-orange?style=for-the-badge)
![Theme](https://img.shields.io/badge/Theme-Blockchain%20%26%20Cybersecurity-purple?style=for-the-badge)

A centralized, tamper-proof, and intelligent digital platform designed for storing, managing, searching, verifying, and sharing sensitive legal and investigative documents (FIRs, charge sheets, forensic reports, court filings, witness statements, and evidence records).

---

## 📌 Team Details

- **Team ID:** `152130`
- **Team Name:** `BlockCoders`
- **Project Title:** Secure Digital Document Management System for Legal & Investigation Documents

---

## 📖 Table of Contents

- [Problem Overview](#-problem-overview)
- [Proposed Solution](#-proposed-solution)
- [Key Features](#-key-features)
- [System Architecture & Workflow](#-system-architecture--workflow)
- [Tech Stack](#-tech-stack)
- [Security & Compliance](#-security--compliance)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Environment Configuration](#environment-configuration)
  - [Installation & Local Setup](#installation--local-setup)
- [Target Users & Stakeholders](#-target-users--stakeholders)
- [Competitive Advantages](#-competitive-advantages)
- [References](#-references)

---

## 🚨 Problem Overview

Existing solutions across the Indian criminal justice and judicial ecosystem face key operational limitations:
- **CCTNS:** Primarily state-siloed databases with limited cross-state or document-level synchronization.
- **e-Courts & NJDG:** Highly focused on case status/metadata and scanned PDFs rather than full evidence chain-of-custody.
- **ICJS:** Links systems via centralized government cloud services without decentralized, independent cryptographic proof of document integrity.
- **e-Sakshya:** Evidence chain-of-custody across police, forensics, and prosecution often relies on manual logs, vulnerable to tampering or loss.

---

## 💡 Proposed Solution

Our platform introduces a permissioned blockchain layer coupled with modern AI, strong cryptographic encryption, and a strict Role-Based Access Control (RBAC) model. 

It provides an immutable audit trail (`Who` $\rightarrow$ `What` $\rightarrow$ `When` $\rightarrow$ `Action`), cryptographic tamper detection via document hashing, automated AI-driven OCR and classification, and semantic vector search across full document contents.

---

## ✨ Key Features

- **Centralized Document Vault:** Unified repository for FIRs, charge sheets, forensic reports, case filings, and witness testimonies.
- **Blockchain-Anchored Integrity:** SHA-256 document hashes recorded on an immutable ledger for instant tamper detection.
- **End-to-End Encryption:** Documents encrypted with AES-256-GCM before persistent cloud/IPFS storage.
- **AI-Powered OCR & Semantic Search:** Integrated PaddleOCR and pgvector-based embeddings (Sentence Transformers + RAG) for natural language and content-aware document discovery.
- **Automated Chain-of-Custody:** Every custody transfer, review, export, or edit generates an immutable audit record.
- **Digital Signatures:** Cryptographic signing for authenticating department officers and official handovers.
- **Interoperability:** Modular microservice architecture designed to complement existing ICJS, CCTNS, and e-Courts workflows.

---

## 🔄 System Architecture & Workflow

The lifecycle of every legal record uploaded to the system follows an end-to-end 12-step secure pipeline:

```
[1] User Login
      │
[2] Identity Verification + MFA
      │
[3] Role-Based Access Control (RBAC)
      │
[4] Document Upload
      │
[5] Encryption (AES-256-GCM)
      │
[6] AI Classification & Metadata Extraction (PaddleOCR / LLM)
      │
[7] Secure Storage (Cloud / MinIO / IPFS)
      │
[8] Cryptographic Hash Generation (SHA-256)
      │
[9] Hash Stored on Permissioned Blockchain
      │
[10] Digital Signature & Verification
      │
[11] Tamper-Proof Audit Log Generation
      │
[12] Authorized Search, View, and Secure Inter-Department Sharing
```

---

## 🛠 Tech Stack

### **Frontend**
- **Framework:** React.js
- **Language:** TypeScript
- **Styling:** Tailwind CSS / Modern Component UI
- **Features:** Role-specific dashboards, secure previewer, audit timeline visualization

### **Backend**
- **Runtime & Framework:** Python 3.12+ with FastAPI
- **Database ORM:** SQLAlchemy
- **Architecture:** Asynchronous RESTful Microservices

### **Database & Persistent Storage**
- **Relational DB:** PostgreSQL (Users, Cases, and Document Metadata)
- **Vector Search Engine:** `pgvector` (Vector embeddings for semantic RAG search)
- **Object Storage:** MinIO / IPFS (Encrypted document blobs)
- **Cache & Queue:** Redis (Task queues, session caching, async background jobs)

### **Security & Cryptography**
- **Authentication:** JWT + Multi-Factor Authentication (MFA)
- **Authorization:** Granular Role-Based Access Control (RBAC)
- **Password Security:** Argon2id
- **At-Rest Encryption:** AES-256-GCM
- **Integrity Anchoring:** SHA-256 cryptographic hashes on Permissioned Blockchain
- **Authenticity:** PKI-based Digital Signatures

### **AI & Document Intelligence**
- **OCR Engine:** PaddleOCR & OpenCV (Scanned records and image processing)
- **Embeddings:** Sentence Transformers
- **Retrieval Engine:** Retrieval-Augmented Generation (RAG) + LLM for conversational case document querying

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18.x or later)
- [Python](https://www.python.org/) (v3.12 or later)
- [Docker](https://www.docker.com/) & Docker Compose
- [PostgreSQL](https://www.postgresql.org/) with `pgvector` extension enabled

---

### Environment Configuration

Create a `.env` file in the root directory:

```env
# Application
SECRET_KEY=your_super_secret_jwt_key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=60

# Database
DATABASE_URL=postgresql+asyncpg://postgres:password@localhost:5432/sih_legal_docs

# Redis
REDIS_URL=redis://localhost:6379/0

# MinIO / Object Storage
MINIO_ENDPOINT=localhost:9000
MINIO_ACCESS_KEY=minioadmin
MINIO_SECRET_KEY=minioadmin
MINIO_BUCKET=legal-documents

# Encryption
DOCUMENT_ENCRYPTION_KEY=32_byte_base64_encoded_key_here

# Blockchain / Node RPC
BLOCKCHAIN_RPC_URL=http://localhost:8545
CONTRACT_ADDRESS=0xYourDeployedContractAddress
```

---

### Installation & Local Setup

#### 1. Clone the Repository
```bash
git clone https://github.com/BlockCoders/secure-legal-dms.git
cd secure-legal-dms
```

#### 2. Start Services via Docker Compose
```bash
docker-compose up -d postgres redis minio
```

#### 3. Backend Setup
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run migrations
alembic upgrade head

# Start FastAPI server
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

#### 4. Frontend Setup
```bash
cd ../frontend
npm install
npm run dev
```

The application will be accessible at `http://localhost:5173` with documentation available at `http://localhost:8000/docs`.

---

## 👥 Target Users & Stakeholders

| Stakeholder | Key Role & Access Privileges |
| :--- | :--- |
| **Police Departments** | Upload initial FIRs, attach seizure memos, and manage case evidence |
| **Investigation Officers** | Review case files, cross-examine statements, append charge sheets |
| **Forensic Laboratories** | Securely submit forensic reports with tamper-proof chain of custody |
| **Courts & Judges** | Review validated, immutable evidence records and verify digital signatures |
| **Legal Counsel / Prosecutors** | Access authorized discovery documents and file digital motions |

---

## 🏆 Competitive Advantages

| Feature | Legacy Systems (CCTNS / e-Courts) | Our Solution (BlockCoders) |
| :--- | :--- | :--- |
| **Tamper Verification** | Centralized database logs (alterable) | Blockchain-anchored SHA-256 hashes |
| **Search Capabilities** | Keyword / Case ID lookup only | AI OCR + Semantic Vector Search (RAG) |
| **Chain-of-Custody** | Mixed physical files & manual registry | Automated on-chain custody handoffs |
| **Access Control** | Departmental silos | Unified RBAC with cryptographic verification |

---

## 📚 References

1. **Verma, A., Bhattacharya, P., Saraswat, D., & Tanwar, S. (2021).** *NyaYa: Blockchain-based electronic law record management scheme for judicial investigations.* Journal of Information Security and Applications, 63, 103025.
2. **Tasnim, M. A., Omar, A. A., Rahman, M. S., & Bhuiyan, M. Z. A. (2018).** *CRAB: Blockchain-based criminal record management system.* SpaCCS 2018, Springer.
3. **Batista, D. et al. (2023).** *Exploring Blockchain Technology for Chain of Custody Control in Physical Evidence: A Systematic Literature Review.* Journal of Risk and Financial Management, 16(8), 360.
4. **Lemieux, V. L. (2021).** *Blockchain and Recordkeeping.* Computers, 10(11), 135.
5. **Government Initiatives:** Inter-Operable Criminal Justice System (ICJS), Crime and Criminal Tracking Network and Systems (CCTNS), and e-Courts Mission Mode Project (NJDG).

---

## 📄 License

This project is developed under the **Smart India Hackathon (SIH 2026)** framework. Licensed under the [MIT License](LICENSE).