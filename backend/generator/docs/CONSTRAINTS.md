# Current Constraints: MCP Server Generator

This document lists the current implemented limits of the MCP Server Generator module.

## Scope Boundary

This documentation covers only the core generator logic. It does not include external UI components, orchestration APIs, or independent policy engines.

## Generator Mode

The current generator uses deterministic templates to ensure reliability and safety.

**Implemented:**
- Generation of complete MCP server project structures.
- Automated creation of tool metadata, dependency lists, and risk assessments.
- Standardized project naming and tool normalization.

**Not Implemented:**
- Arbitrary AI-driven code synthesis (planned for future versions).
- Multi-language generation (currently TypeScript only).

## MCP Transport

**Implemented:**
- Generated servers use the standard MCP `stdio` transport.

**Planned / Roadmap:**
- HTTP/SSE (Server-Sent Events) transport support for cloud deployments (IBM Code Engine).
- Custom transport protocol extensions.

## Template System

**Implemented:**
- Static TypeScript string-literal templates.
- Enforcement of `@modelcontextprotocol/sdk` standards.

**Current Limitation:**
- Templates are hardcoded; modifications require structural updates to the generator engine.

## Security and Compliance

**Implemented in Output:**
- Mandatory environment-based configuration.
- Prevention of hardcoded secrets.
- Strict input validation via Zod.
- PII-safe logging (masking sensitive data).
- Execution timeouts and conservative error handling.

## Ownership and Integration

This module is designed to be consumed as a standalone library or part of a larger automation pipeline. Integration changes should be coordinated with the module's public API.