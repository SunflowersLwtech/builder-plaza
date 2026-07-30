<div align="center">

<img src="./images/Builder_Plaza_Logo.png" alt="Builder Plaza logo" width="160">

# Builder Plaza

**Mobile App Engineering (MAE) Group Assignment Submission**

[![CI](https://github.com/SunflowersLwtech/builder-plaza/actions/workflows/ci.yml/badge.svg)](https://github.com/SunflowersLwtech/builder-plaza/actions/workflows/ci.yml)
![Flutter](https://img.shields.io/badge/Flutter-iOS%20%2B%20Android-02569B?logo=flutter&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Python%203.12-009688?logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-4169E1?logo=postgresql&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-ECS%20Fargate-FF9900?logo=amazonwebservices&logoColor=white)

</div>

---

### 📋 Assignment & Team Submission Details

| Item | Details |
| --- | --- |
| **Module** | CT124-3-2-MAE Mobile App Engineering |
| **Project Title** | Builder Plaza |
| **Lecturer** | Mr. Amad Arshad |
| **Intake Code** | APD2F2511CS / APU2F2511CS |
| **Team Members** | • **Choong Ti Huai** (TP078539)<br>• **Liu Wei** (TP085412)<br>• **Gao Xing** (TP085905) |
| **Live API Backend** | `http://builder-plaza-alb-270417897.ap-southeast-1.elb.amazonaws.com` |
| **Demo Dashboard (Grafana)** | `http://builder-plaza-alb-270417897.ap-southeast-1.elb.amazonaws.com:8080` |

---

## Executive Summary

Collaboration trust today is scattered across GitHub (proof of work), LinkedIn (professional identity), and Discord / Product Hunt (weak availability signals). **Builder Plaza** unifies them into a single structured layer that answers three questions about any potential collaborator:

> **Is this person credible? Are they open to collaborating right now? Is it safe to contact them in this specific context?**

---

## Key Modules & Features

| Module | Features & Logic |
| --- | --- |
| **Dual-Source Trust Gateway** | Sign in with GitHub OAuth **and** LinkedIn OIDC — identity is anchored to both work and career graphs |
| **Profile & Completeness** | Structured builder profile with real-time completeness scoring |
| **Project Cards** | CRUD project showcases with S3-hosted screenshots and demo links |
| **Growth Plaza** | GitHub activity polling summarised by AWS Bedrock into readable growth updates |
| **Discovery & Matching** | Semantic skill matching via SBERT embeddings stored in `pgvector`, ranked by Gaussian Process Regressor |
| **Trust Score** | Composite credibility signal computed from verified activity & peer reviews |
| **Controlled DM** | Contextually gated messaging — zero spam inboxes |
| **Request Market** | Post and browse concrete collaboration asks with structured role postings |
| **Verified Activity Timeline** | Tamper-evident feed of verified builder code activity |

---

## Architecture & System Design

```
┌─────────────────────────┐         ┌──────────────────────────────────────┐
│   Flutter App           │  HTTP   │  AWS Cloud Infrastructure            │
│   (iOS + Android,       │ ──────► │  ALB ─► ECS Fargate (FastAPI)        │
│    single codebase)     │         │        ├─ RDS Postgres + pgvector    │
│                         │         │        ├─ S3 (Screenshots)           │
└─────────────────────────┘         │        └─ Bedrock (Summarisation)    │
                                    └──────────────────────────────────────┘
```

- **Frontend**: Flutter (Dart), cross-platform for iOS and Android (`frontend/`)
- **Backend**: Python 3.12, FastAPI + SQLAlchemy + Alembic (`backend/`)
- **Matching Engine**: SBERT sentence embeddings in `pgvector`, scored with `GaussianProcessRegressor`
- **Infrastructure**: Containerized on AWS ECS Fargate behind an Application Load Balancer

---

## Repository Structure

```
frontend/    Flutter app — screens, state, widgets, integration_test/
backend/     FastAPI app — routers, services, models, alembic/, tests/
docs/        Documentation, DEPLOYMENT.md, test-evidence/, and demo scripts
tools/       Developer CLI scripts and automated testing tools
.github/     CI pipeline, Android E2E scripts, AWS Device Farm workflow
```

---

## Getting Started

### 1. Backend Setup

```bash
cd backend
python3.12 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt -r requirements-dev.txt
cp ../.env.example ../.env          # fill in OAuth + AWS settings
docker compose up -d                # Postgres + pgvector container
alembic upgrade head && python seed.py
uvicorn app.main:app --reload
```

### 2. Frontend Setup

```bash
cd frontend
flutter pub get
flutter run --dart-define=API_BASE_URL=http://localhost:8000
```

---

## Comprehensive Test Pyramid

The CI pipeline runs the complete test pyramid on every push:

| Tier | Coverage & Verification |
| --- | --- |
| **Backend** | 179 pytest cases (unit + API integration tests against Postgres + `pgvector`) |
| **Frontend** | `flutter analyze` + widget and model tests in `frontend/test/` |
| **Device E2E** | Scripted end-to-end flows on an Android 14 emulator against a live seeded backend + UI Monkey crash crawl |
| **Real Hardware** | On-demand [AWS Device Farm workflow](.github/workflows/device-farm.yml) on physical devices |

Green builds on `main` automatically publish release APKs to [GitHub Releases](https://github.com/SunflowersLwtech/builder-plaza/releases).

---

## Documentation Links

- 📖 [Deployment & AWS Architecture](docs/DEPLOYMENT.md)
- 📊 [Test Evidence & Crawl Logs](docs/test-evidence/)
- 🎬 [Demo Script](docs/DEMO_SCRIPT.md)

---

<div align="center">
Built for MAE Mobile App Engineering Assignment by <b>Choong Ti Huai, Liu Wei, and Gao Xing</b>
</div>
