# Quickstart: MCP Server Generator

Guide for installing, testing, and running the MCP Server Generator.

---

## Installation

```bash
cd backend/generator
npm install
```

> **Windows Users:** If you encounter peer dependency errors with `@modelcontextprotocol/sdk`, use:
> ```bash
> npm install --legacy-peer-deps
> ```

---

## Running Tests

```bash
npm test
```

To run in watch mode during development:
```bash
npm run test:watch
```

---

## Generating an MCP Server

To generate the standard demo server:

```bash
npm run generate:demo
```

**Output location:** `generated/customer-lookup-mcp/`

**Generated Project Structure:**
- `package.json`: Project metadata and dependencies.
- `tsconfig.json`: TypeScript configuration.
- `src/server.ts`: Core MCP server implementation.
- `.env.example`: Environment configuration template.
- `README.md`: Instructions for the generated server.
- `Dockerfile`: Deployment containerization.
- `manifest.json`: MCP protocol manifest.

---

## Inspecting the Generated Project

### 1. Build and Run
```bash
cd generated/customer-lookup-mcp
npm install
npm run build
npm start
```

### 2. Test with MCP Inspector
You can use the official MCP Inspector to test the generated server:
```bash
npx @modelcontextprotocol/inspector
```

---

## Security Features

The generator automatically applies these security patterns:
- **Environment Variables**: No sensitive data is hardcoded.
- **Data Masking**: Logging avoids PII (Personal Identifiable Information).
- **Validation**: Strict input schema validation using Zod.
- **Timeouts**: Built-in safeguards against long-running operations.

---

## Reference
- **SDK**: `@modelcontextprotocol/sdk`
- **Protocol**: [Model Context Protocol](https://modelcontextprotocol.io)
