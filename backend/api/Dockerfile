# syntax=docker/dockerfile:1
FROM node:20-alpine AS base
WORKDIR /repo

# ── Deps for API + generator + policy-engine ──────────────────────────────────
# Copy package files for all three packages first for better layer caching
COPY backend/api/package.json     backend/api/package.json
COPY backend/generator/package.json backend/generator/package.json
COPY backend/policy-engine/package.json backend/policy-engine/package.json

# Install API deps
RUN cd backend/api && npm install

# Install generator deps
RUN cd backend/generator && npm install

# Install policy-engine deps
RUN cd backend/policy-engine && npm install

# ── Full source ────────────────────────────────────────────────────────────────
# Copy repo root + all backend source (complianceWrapper.ts needs generator + policy-engine sources)
COPY . .

WORKDIR /repo/backend/api

# ── Non-root user ─────────────────────────────────────────────────────────────
USER node

EXPOSE 3001

ENV NODE_ENV=development
ENV PORT=3001

# Start the API — complianceWrapper spawns npx tsx at runtime
CMD ["node", "src/server.js"]
