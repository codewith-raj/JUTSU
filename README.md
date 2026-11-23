# JUTSU

**Enterprise Memory OS · Corporate memory, connected to its evidence.**

[Visit jutsu.co.in](https://jutsu.co.in/)

JUTSU is an enterprise memory platform that turns an organisation’s scattered knowledge into a searchable, evidence-backed memory. It is designed to connect read-only to the tools teams already use, link people, projects, decisions and events in a temporal knowledge graph, and make answers traceable to their original sources.

> A fact is only useful when you can see where it came from—and only visible when you have permission to see that evidence.

## What JUTSU is being built to do

- **Cited Q&A:** Answer questions from organisational knowledge with links back to supporting evidence.
- **Decision history:** Find what was decided, when, by whom and what changed later.
- **Expert discovery:** Find people with demonstrated experience on a topic or project.
- **Knowledge risk:** Reveal topics and projects where critical know-how is concentrated.
- **Handover preparation:** Assemble useful, source-backed context for transitions.
- **Guided onboarding:** Help people find the most relevant material to read first.

These are the product’s intended surfaces; implementation is actively evolving.

## Project overview

This repository contains the JUTSU web application, API, background worker and shared packages:

| Path | Purpose |
| --- | --- |
| `apps/web` | Next.js web application |
| `apps/api` | FastAPI service |
| `apps/worker` | Background ingestion and processing jobs |
| `packages/` | Shared Python and UI packages |
| `infra/docker` | Local Postgres, Neo4j and Redis services |
| `infra/terraform` | Infrastructure configuration |
| `docs/` | Product specification, plans, runbooks and deployment notes |

The project uses Next.js and React for the web app, Python 3.12 and FastAPI for the service layer, PostgreSQL with pgvector for relational and vector data, Neo4j for graph data, and Redis for the local job queue.

## Prerequisites

- Node.js 22 or newer and pnpm 11.5.1
- Python 3.12 and [uv](https://docs.astral.sh/uv/)
- Docker with Docker Compose
- GNU Make (useful for the documented commands)

## Run locally

1. Clone the repository and install dependencies:

   ```bash
   git clone https://github.com/codewith-raj/JUTSU.git
   cd JUTSU
   pnpm install
   uv sync --all-packages --all-groups
   ```

2. Create your local environment file:

   ```bash
   cp .env.example .env
   ```

   Set a unique `JUTSU_EMAIL_PEPPER` in `.env`. Generate one with:

   ```bash
   python -c "import secrets; print(secrets.token_urlsafe(32))"
   ```

   The example file contains development-only database credentials and local service URLs. Configure mail or Google Cloud credentials only if your workflow needs them. Never commit `.env` or production secrets.

3. Start the local data services and apply database migrations:

   ```bash
   make up
   make migrate
   ```

4. Start the app components in separate terminals:

   ```bash
   make dev      # Web app: http://localhost:3210
   make api      # API: http://localhost:8000
   make worker   # Background worker
   ```

To stop the local data services, run `make down`.

## Common commands

| Command | Description |
| --- | --- |
| `make up` / `make down` | Start or stop local Postgres, Neo4j and Redis |
| `make migrate` | Apply Postgres migrations |
| `make dev` | Start the web app on port 3210 |
| `make api` | Start the FastAPI service on port 8000 |
| `make worker` | Start the background worker |
| `make preflight` | Run lint, type checks and test checks before a change |
| `pnpm build` | Build the web app |

For deployment and operational details, see [`docs/deploy.md`](docs/deploy.md). The product and architecture specification is in [`docs/jutsu-master-spec.md`](docs/jutsu-master-spec.md).

## Contributing

Changes are welcome. Start by reviewing the project instructions in [`AGENTS.md`](AGENTS.md) and the relevant design notes in `docs/`. Keep source evidence and access controls intact when changing knowledge retrieval or graph behavior. Run `make preflight` before submitting a change.

## About

JUTSU is Team Code Hail Mary’s enterprise memory capstone project at Manipal University Jaipur, developed in collaboration with Deloitte.

**Website:** [jutsu.co.in](https://jutsu.co.in/) · **Repository:** [codewith-raj/JUTSU](https://github.com/codewith-raj/JUTSU)
