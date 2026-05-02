# Commands: MCP Server Generator

This document lists the available commands for developing and operating the MCP Server Generator.

## Working Directory

All commands should be executed from the module root:

```bash
cd backend/generator
```

## Setup and Maintenance

### Install Dependencies
```bash
npm install
```
*Use `npm install --legacy-peer-deps` on Windows if necessary.*

### Type Checking
```bash
npm run typecheck
```
Validates TypeScript types without emitting files.

---

## Development and Testing

### Run Tests
```bash
npm test
```
Executes the test suite via Vitest.

### Development Mode (Watch)
```bash
npm run test:watch
```

---

## Build and Distribution

### Build Generator
```bash
npm run build
```
Compiles the generator package to the `dist/` directory.

### Generate Sample Server
```bash
npm run generate:demo
```
Creates a sample MCP server project in `generated/customer-lookup-mcp/`.

---

## Operating Generated Servers

After running the generator, you can operate the output project:

```bash
cd generated/customer-lookup-mcp
npm install
npm run build
npm start
```

The generated server uses `stdio` transport. Use the [MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector) for debugging.