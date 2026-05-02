# syntax=docker/dockerfile:1
FROM node:20-alpine

WORKDIR /app

# Copy package files
COPY package.json ./

# Install deps
RUN npm install

# Copy source
COPY . .

# Non-root user
USER node

# MCP stdio service entrypoint
CMD ["npx", "tsx", "src/index.ts"]
