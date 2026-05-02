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

# Demo entrypoint: runs the generator demo
CMD ["npx", "tsx", "src/demo.ts"]
