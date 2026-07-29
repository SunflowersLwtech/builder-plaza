<div align="center">

<img src="./images/Builder_Plaza_Logo.png" alt="Builder Plaza logo" width="160">

# Builder Plaza

**A trust layer for finding collaborators — verified proof of work, professional identity, live collaboration intent, and safe contextual outreach, in one app.**

[![CI](https://github.com/SunflowersLwtech/builder-plaza/actions/workflows/ci.yml/badge.svg)](https://github.com/SunflowersLwtech/builder-plaza/actions/workflows/ci.yml)
![Flutter](https://img.shields.io/badge/Flutter-iOS%20%2B%20Android-02569B?logo=flutter&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Python%203.12-009688?logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-4169E1?logo=postgresql&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-ECS%20Fargate-FF9900?logo=amazonwebservices&logoColor=white)

</div>

---

Collaboration trust today is scattered across GitHub (proof of work), LinkedIn (professional identity), and Discord / Product Hunt (weak availability signals). Builder Plaza unifies them into a single structured layer that answers three questions about any potential collaborator:

> **Is this person credible? Are they open to collaborating right now? Is it safe to contact them in this specific context?**

## Features

| Module | What it does |
| --- | --- |
| **Dual-Source Trust Gateway** | Sign in with GitHub OAuth **and** LinkedIn OIDC — identity is anchored to both work and career graphs |
| **Profile & Completeness** | A structured builder profile with a live completeness score |
| **Project Cards** | CRUD project showcases with S3-hosted screenshots and demo links |
| **Growth Plaza** | GitHub activity polling summarised by AWS Bedrock into readable growth updates |
| **Discovery & Matching** | Semantic skill matching: SBERT embeddings in pgvector, ranked by a Gaussian Process Regressor |
| **Trust Score** | A composite credibility signal computed from verified activity |
| **Controlled DM** | Contact is gated by context — no cold-spam inbox |
| **Request Market** | Post and browse concrete collaboration asks with role postings |
| **Verified Activity Timeline** | A tamper-evident feed of what a builder actually shipped |

## Architecture

```
┌─────────────────────────┐         ┌──────────────────────────────────────┐
│   Flutter app           │  HTTPS  │  AWS                                 │
│   (iOS + Android,       │ ──────► │  ALB ─► ECS Fargate (FastAPI)        │
│    single codebase)     │         │        ├─ RDS Postgres + pgvector    │
└─────────────────────────┘         │        ├─ S3 (screenshots)           │
                                    │        └─ Bedrock (summarisation)    │
                                    └──────────────────────────────────────┘
```

- **Frontend** — Flutter (Dart), one codebase for iOS and Android: `frontend/`
- **Backend** — Python 3.12, FastAPI + SQLAlchemy + Alembic: `backend/`
- **Matching engine** — SBERT sentence embeddings stored in pgvector, scored with scikit-learn's `GaussianProcessRegressor` ([ADR-0001](docs/adr/0001-matching-engine-sbert-gpr.md))
- **Infrastructure** — Docker image on ECS Fargate behind an ALB ([ADR-0002](docs/adr/0002-backend-python-fastapi-aws.md))

## Repository layout

```
frontend/    Flutter app — screens, state, widgets, integration_test/
backend/     FastAPI app — routers, services, models, alembic/, tests/
docs/        ADRs, deployment guide, user manual, project report
.github/     CI pipeline, Android E2E scripts, AWS Device Farm workflow
PRD.md       Product requirements (modules F1–F10)
```

## Getting started

### Backend

```bash
cd backend
python3.12 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt -r requirements-dev.txt
cp ../.env.example ../.env          # fill in OAuth + AWS settings
docker compose up -d                # throwaway Postgres + pgvector for tests
alembic upgrade head && python seed.py
uvicorn app.main:app --reload
```

### Frontend

```bash
cd frontend
flutter pub get
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000   # Android emulator
```

See [`docs/DEPLOYMENT.md`](docs/DEPLOYMENT.md) for the AWS deployment guide and [`docs/USER_MANUAL.md`](docs/USER_MANUAL.md) for the end-user walkthrough.

## Testing

The CI pipeline runs the full test pyramid on every push:

| Tier | What runs |
| --- | --- |
| Backend | 171 pytest cases — 141 unit tests plus 30 API integration tests against a real Postgres + pgvector service container |
| Frontend | `flutter analyze` plus widget and model tests |
| Device E2E | Scripted end-to-end flows on an Android 14 emulator against a live seeded backend, followed by a UI Exerciser Monkey crash crawl |
| Real hardware | An on-demand [AWS Device Farm workflow](.github/workflows/device-farm.yml) runs the same flows on physical Samsung devices |

Green builds on `main` automatically publish release APKs to [GitHub Releases](https://github.com/SunflowersLwtech/builder-plaza/releases).

## Documentation

- [Product Requirements](PRD.md) — full product scope, ERD, and Live/Mocked tiering
- [Architecture Decision Records](docs/adr/) — why SBERT + GPR, why FastAPI on Fargate
- [Deployment Guide](docs/DEPLOYMENT.md) — ECS Fargate, RDS, ALB setup
- [Project Report](docs/REPORT.md) — current implementation status

---

<div align="center">
Built with Flutter & FastAPI by <a href="https://github.com/SunflowersLwtech">LiuWei</a>
</div>
