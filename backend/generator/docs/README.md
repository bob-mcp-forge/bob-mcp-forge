# MCP Server Generator

**Module:** `backend/generator`  
**Docs:** [Architecture](./ARCHITECTURE.md) | [Commands](./COMMANDS.md) | [Usage Manual](./USAGE_MANUAL.md) | [Constraints](./CONSTRAINTS.md)

An automated engine that converts natural language tool descriptions into complete, production-ready Model Context Protocol (MCP) server projects.

## Purpose

This module simplifies the creation of MCP servers by automating the scaffolding and implementation logic. Given a description of a tool, it outputs a complete TypeScript project featuring:

- **Official SDK Integration**: Built on `@modelcontextprotocol/sdk`.
- **Input Validation**: Automatic Zod schema generation.
- **Security Best Practices**: Environment configuration, PII-safe logging, and secret prevention.
- **Ready for Deployment**: Includes Docker and manifest files.

---

## Installation

```bash
cd backend/generator
npm install
```

*Windows users: use `npm install --legacy-peer-deps` if peer dependency issues occur.*

---

## Core Scripts

| Script | Description |
|--------|-------------|
| `npm test` | Run the validation suite |
| `npm run build` | Compile the generator |
| `npm run generate:demo` | Generate a sample MCP server |

---

## Programmatic API

```typescript
import { generateMcpServer } from './src/index.js';

const output = generateMcpServer({
  description: 'Tool that looks up customer records by email',
});
```

---

## Technology Stack
- **Language**: TypeScript (ESM)
- **SDK**: Model Context Protocol SDK
- **Validation**: Zod
- **Testing**: Vitest

## Integration
For detailed instructions on how to connect your generated servers to clients like **Antigravity** or **Claude Desktop**, refer to the [Usage Manual](./USAGE_MANUAL.md#6-connecting-to-an-mcp-host).
