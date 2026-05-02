# ─────────────────────────────────────────────────────────────────────────────
# Bob MCP Forge — Docker Deployment
# ─────────────────────────────────────────────────────────────────────────────

## Overview

Bob MCP Forge ships with Docker Compose for local development, demos, and production deployments. All images are built from the repo — no external registry required.

## Services

| Service | Port | Description | Profile |
|---------|------|-------------|---------|
| `frontend` | 8080 | Static HTML/CSS served via nginx | default |
| `api` | 3001 | Express REST API orchestrator | default |
| `mongo` | 27017 | MongoDB for local job persistence | default |
| `policy-engine` | — | Standalone MCP stdio service | profile:tools |
| `generator` | — | Generator demo/tooling CLI | profile:tools |

## Quick Start

```bash
# Start the app (frontend + API + Mongo)
docker compose up

# Start with optional tooling services (standalone policy-engine + generator)
docker compose --profile tools up

# Stop everything
docker compose down

# Rebuild after changes
docker compose build
docker compose up
```

Open:

- Frontend: http://localhost:8080
- API health: http://localhost:3001/health

Test the main flow:

```bash
curl -X POST http://localhost:3001/api/v1/generate \
  -H "Content-Type: application/json" \
  -d '{"description":"Tool that stores api key","complianceProfile":"soc2"}'
```

The response includes `jobId`, `generatedFiles`, and `complianceReport`. Poll the job:

```bash
curl http://localhost:3001/api/v1/jobs/<jobId>
```

Expected completed job state: `status: "generated"`, `progress: 100`, generated code stored under `data.generatedCode`, and audit output under `data.complianceReport`.

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `NODE_ENV` | `development` | Runtime environment |
| `PORT` | `3001` | API server port |
| `MONGODB_URI` | *(optional)* | MongoDB connection string |
| `BOB_API_KEY` | *(unset)* | IBM Bob API key, once available |
| `BOB_API_URL` | `https://bob.ibm.com` | IBM Bob endpoint, once confirmed |

> **Note:** Compose provides MongoDB by default. The API also works in dev mode without `MONGODB_URI` — it falls back to an in-memory mock store when no Mongo URI is provided.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      docker compose                         │
├──────────┬──────────┬──────────┬───────────┬───────────────┤
│ frontend │   api   │  mongo   │  policy-   │   generator   │
│  (nginx) │ (Node)  │  (Mongo) │  engine   │    (Node)     │
│   :8080  │  :3001  │  :27017  │  stdio    │     CLI       │
└──────────┴──────────┴──────────┴───────────┴───────────────┘
```

The **api** service spawns the generator and policy-engine via `npx tsx` at runtime — no separate process management needed for the normal flow. The standalone **policy-engine** and **generator** services are for dev/testing only.

## Development

### Running a single service

```bash
docker compose up api     # API only
docker compose up frontend # Frontend only
```

### Viewing logs

```bash
docker compose logs -f api
docker compose logs -f frontend
```

### Accessing containers

```bash
docker compose exec api sh
docker compose exec mongo mongosh
```

### Local Mongo vs mock mode

`docker compose up` starts MongoDB and passes `MONGODB_URI=mongodb://mongo:27017/bob-mcp-forge` when configured through `.env`. If `MONGODB_URI` is omitted in development, the API falls back to an in-memory mock store. That is useful for quick demos, but data disappears when the process restarts.

Production should set:

```bash
NODE_ENV=production
MONGODB_URI=<real MongoDB connection string>
```

In production, missing `MONGODB_URI` is a hard failure by design.

## Deployment Checklist

Before deploying to IBM Cloud Code Engine or another container runtime:

- Set `NODE_ENV=production`.
- Set `MONGODB_URI` to MongoDB Atlas or managed Mongo.
- Set IBM Bob credentials when available (`BOB_API_KEY`, `BOB_API_URL`).
- Expose API container port `3001`.
- Expose frontend container port `80` or map it to the platform default.
- Configure frontend API base URL to point at the deployed API.
- Run tests before image build: `npm test` in `backend/api` and `backend/policy-engine`.

## Troubleshooting

### `MONGODB_URI is not defined`

In development this should only warn and continue in mock mode. In production it exits intentionally. Set `MONGODB_URI` for deployed environments.

### `Route POST /api/v1/generate not found`

Pull latest main. The API must mount the generate router with `app.use('/api/v1/generate', generateRoute)`.

### Frontend keeps polling forever

Pull latest main. The generate endpoint should mark jobs as `generated` with `progress: 100` after generation and audit complete.

### Port already in use

Override ports in compose or stop the local process using the port. API defaults to `3001`; frontend host port defaults to `8080`.

## Docker Files Reference

| File | Purpose |
|------|---------|
| `frontend/Dockerfile` | nginx:alpine serving static HTML/CSS |
| `backend/api/Dockerfile` | Node 20 with API + generator + policy-engine sources |
| `backend/policy-engine/Dockerfile` | Standalone MCP stdio service image |
| `backend/generator/Dockerfile` | Generator demo/tooling CLI image |
| `docker-compose.yml` | Multi-service compose with profiles |
| `.dockerignore` | Prevents node_modules/.git from copying into images |

## Building

> **Do not build during apply phase** — this is handled by the CI/CD pipeline or explicit user request.

```bash
# Manual build (when authorized)
docker compose build
docker compose up
```

## Notes

- All services run as non-root user inside containers.
- The API container includes the full repo at `/repo` so `complianceWrapper.ts` path resolution works at runtime via `npx tsx`.
- Frontend is pure static HTML — no build step, no Node.js needed.
- MongoDB is volume-persisted at `mongo_data` for local development.
