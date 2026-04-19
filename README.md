# Trevonti HBYS

<div align="center">

![Trevonti HBYS](https://img.shields.io/badge/Trevonti-HBYS-0EA5A0?style=for-the-badge&logo=hospital&logoColor=white)
![Version](https://img.shields.io/badge/version-1.0.0-blue?style=for-the-badge)
![License](https://img.shields.io/badge/license-Proprietary-red?style=for-the-badge)
![FHIR R4](https://img.shields.io/badge/FHIR-R4%20Compliant-orange?style=for-the-badge)
![KVKK](https://img.shields.io/badge/KVKK-Compliant-green?style=for-the-badge)

**Turkey's next-generation, cloud-native Hospital Information Management System**

*Replacing legacy on-premise HIMS with AI-powered, fully integrated SaaS*

[📋 Documentation](#documentation) · [🏗 Architecture](#architecture) · [🤖 AI Engine](#ai-engine) · [🔗 Integrations](#integrations)

</div>

---

## 🏥 What is Trevonti HBYS?

Trevonti HBYS is a comprehensive Hospital Information Management System (HIMS) designed specifically for Turkey's healthcare sector. It replaces 10–20 year old, server-based legacy systems with a modern, cloud-native, AI-augmented platform that runs entirely in the browser — no installation required on hospital workstations.

### The Problem We Solve

| Problem | Impact |
|---------|--------|
| 78% of Turkish hospitals run outdated on-premise HIMS | No mobile, no AI, no real integration |
| Paper-based medication orders | Drug errors 3× European average |
| Manual SGK billing code entry | Up to 12% revenue loss per hospital |
| Fragmented systems (Lab, Radiology, Pharmacy separate) | No unified patient view |
| No FHIR R4 / e-Nabız integration | Disconnected from national health ecosystem |

---

## ✨ Platform Overview

Trevonti HBYS consists of **21 clinical and administrative modules** across 3 platforms:

### 🖥 Web Application (Next.js 14)
Full-featured clinical workstation experience running in any modern browser on existing Windows 11 hardware.

### 📱 Mobile Application (React Native 0.74)
iOS + Android apps for nurses (BCMA medication safety), physicians (order signing, vital review), and patients (portal, telemedicine).

### 🤖 AI Engine (Python FastAPI)
Dedicated AI microservice for clinical decision support, risk prediction, and Turkish NLP.

---

## 🗂 Module Catalog

<table>
<tr>
<td valign="top">

**🏥 Clinical**
- HIS Core — Patient & Visit Management
- CPOE — Electronic Order Entry
- Clinical Decision Support (CDS)
- SOAP Notes + Turkish Voice Dictation
- OR/ICU + WHO Surgical Safety
- Cardiology Cath Lab
- IVF / Reproductive Medicine
- Oncology Information System (OIS)
- Dialysis Management

</td>
<td valign="top">

**🔬 Diagnostic**
- LIS — Laboratory (HL7 v2.5.1)
- RIS/PACS — OHIF DICOM Viewer
- Blood Bank (ISBT 128)
- Digital Pathology
- AI Sepsis Early Warning
- AI Readmission Prediction
- Turkish NLP Clinical Notes

</td>
<td valign="top">

**💼 Administrative**
- Pharmacy (FEFO + BCMA)
- CSSD Sterilization
- Finance + SGK Medula
- e-Invoice (UBL 2.1 / GİB)
- Supply Chain Management
- Cost Accounting
- Patient Portal + RPM
- Telemedicine (USBS compliant)
- Call Center CTI

</td>
</tr>
</table>

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Client Layer                          │
│   Next.js 14 (Web)  │  React Native 0.74 (iOS/Android)  │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTPS / WebSocket
┌──────────────────────────▼──────────────────────────────┐
│              API Gateway (NestJS 10)                     │
│         Keycloak 24 OIDC · JWT · Rate Limiting           │
└───┬────────┬────────┬────────┬────────┬─────────────────┘
    │        │        │        │        │
  HIS      LIS     Finance   FHIR     AI Engine
  Core    :3003    :3004     R4       FastAPI
  :3001             │        :3008    :8000
    │               │
    └───────────────┴──── Apache Kafka 3.7 ──────┐
                                                  │
┌─────────────────────────────────────────────────▼───────┐
│                    Data Layer                            │
│  PostgreSQL 16    │  Redis 7     │  ClickHouse OLAP      │
│  (Patroni HA)     │  (Sentinel)  │  (Analytics)          │
│  MinIO (Storage)  │  MLflow      │  Orthanc (PACS)       │
└─────────────────────────────────────────────────────────┘
```

### Technology Stack

| Layer | Technology |
|-------|-----------|
| Web Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS |
| Mobile | React Native 0.74, Expo, Offline Sync |
| API Gateway | NestJS 10, Fastify, OpenAPI 3.0 |
| Microservices | 21 × NestJS services, Nx 18 monorepo |
| AI Engine | Python 3.12, FastAPI, scikit-learn, spaCy (Turkish) |
| Message Bus | Apache Kafka 3.7 (KRaft mode) |
| Primary DB | PostgreSQL 16, Prisma ORM, Patroni HA |
| Cache | Redis 7, Sentinel |
| Analytics | ClickHouse 24, MLflow 2.13 |
| PACS | Orthanc + OHIF Viewer, DICOMweb |
| Auth | Keycloak 24, OIDC, MFA, SMART on FHIR |
| Object Storage | MinIO (S3-compatible) |
| Container | Docker, Kubernetes, Helm, Istio mTLS |
| Security | Cilium (zero-trust), AES-256-GCM, Velero |
| CI/CD | GitHub Actions, Trivy, Canary deployment |

---

## 🤖 AI Engine

Our AI Engine runs as a dedicated FastAPI microservice with 4 clinical models:

| Model | Accuracy | Description |
|-------|----------|-------------|
| **Sepsis Early Warning** | 72% detection | qSOFA + NEWS2 hybrid model |
| **30-Day Readmission** | 89% AUC | LACE+ algorithm with SHAP explanations |
| **Turkish Clinical NLP** | 94% accuracy | SOAP note extraction from voice dictation |
| **Drug Interaction CDS** | 99% capture rate | Real-time medication safety alerts |

> See [trevonti-ai-engine](https://github.com/trevonti-hbys/trevonti-ai-engine) for model implementations.

---

## 🔗 Integrations

### Turkish National Health Systems

| System | Status | Description |
|--------|--------|-------------|
| SGK Medula | ✅ Implemented | Insurance provisioning + SUT billing |
| Sağlık-Net | ✅ Implemented | 10 message types (POLKAYIT, TABURCU...) |
| TİTS | ✅ Implemented | Drug tracking barcode system |
| e-Invoice (GİB) | ✅ Implemented | UBL 2.1, XAdES digital signature |
| FHIR R4 / e-Nabız | ✅ Implemented | 7 resources, CapabilityStatement |
| MERNIS (NVI) | 🔄 In Progress | TC identity verification |
| e-Prescription (HKSS) | 📅 Q1 2026 | Electronic prescription |
| TÜRKOD/UKH | 📅 Q4 2025 | National alert services |

### Clinical Standards

| Standard | Status |
|----------|--------|
| HL7 v2.5.1 (LIS) | ✅ |
| FHIR R4 (4.0.1) | ✅ |
| DICOM / DICOMweb | ✅ |
| LOINC (Lab codes) | ✅ |
| ICD-10 (Diagnoses) | ✅ |
| ATC (Medications) | ✅ |
| GS1 UDI (Implants) | ✅ |
| ISBT 128 (Blood Bank) | ✅ |

---

## 🛡 Security & Compliance

- **KVKK Compliant** — Turkey's personal data protection law (GDPR equivalent)
- **AES-256-GCM** — Field-level encryption for sensitive health data
- **Zero-Trust Networking** — Cilium NetworkPolicy, Istio mTLS STRICT
- **Immutable Audit Log** — SHA-256 hash chain, tamper-proof
- **MFA** — Keycloak OTP + biometric (mobile)
- **SİSE Certification** — Target Q2 2026 (Turkey's mandatory HIMS certification)

---

## 📊 Development Status

```
Sprint 01 ✅  HIS Core + CPOE + API Gateway + Next.js 14
Sprint 02 ✅  LIS + HL7 v2.5.1 + Pharmacy FEFO + BCMA
Sprint 03 ✅  Finance + SGK Medula + Sağlık-Net + ERP
Sprint 04 ✅  RIS/PACS + Orthanc DICOMweb + CD Robot
Sprint 05 ✅  OR/ICU + WHO Safety + GS1 UDI + SOFA/qSOFA
Sprint 06 ✅  AI Engine + Sepsis + Turkish NLP + ClickHouse
Sprint 07 ✅  Patient Portal + e-Devlet OIDC + WebRTC + RPM
Sprint 08 ✅  Blood Bank + CSSD + Oncology + Digital Pathology
Sprint 09 ✅  Cardiology Cath Lab + IVF + Dialysis
Sprint 10 ✅  React Native Mobile (iOS+Android) + Biometric
Sprint 11 ✅  HA/DR + K8s Patroni + KVKK/GDPR + SİSE Prep
Sprint 12 ✅  Call Center CTI + SCM + Cost Accounting + Migration
─────────────────────────────────────────────────────
           21 Applications · 12 Prisma Schemas · 200+ Tests
```

---

## 📋 Documentation

| Document | Description |
|----------|-------------|
| [API Reference](./docs/api-reference.md) | OpenAPI 3.0, all endpoints |
| [FHIR R4 Implementation](./docs/fhir-r4.md) | HL7 FHIR R4 compliance guide |
| [Architecture Decision Records](./docs/adr/) | Key technical decisions |
| [Deployment Guide](./docs/deployment.md) | Docker Compose + Kubernetes |

---

## 🎯 Market & Business Model

- **TAM:** ₺18 Billion (Turkish Healthcare IT, 2026)
- **Market Growth:** 23% annually (2024–2028)
- **Business Model:** SaaS subscription ₺25,000–₺120,000/month per hospital
- **Revenue Target:** ₺58M ARR by 2030 (65 hospitals)

---

## 📞 Contact

**Website:** [trevonti.com](https://trevonti.com)
**Email:** info@trevonti.com
**LinkedIn:** [Trevonti HBYS](https://linkedin.com/company/trevonti-hbys)

---

<div align="center">

*Trevonti HBYS — Türkiye'nin Gelecek Nesil Hastane Sistemi*

![Made in Turkey](https://img.shields.io/badge/Made%20in-Turkey%20🇹🇷-red?style=flat-square)
![Healthcare AI](https://img.shields.io/badge/Healthcare-AI%20Powered-0EA5A0?style=flat-square)
![FHIR R4](https://img.shields.io/badge/FHIR-R4%20Ready-orange?style=flat-square)

</div>
