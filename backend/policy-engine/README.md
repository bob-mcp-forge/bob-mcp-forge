# Sovereign Compliance Agent (MCP Server)

Owner: **Anthony**.

This is our differentiator. Built **as an MCP server** so it can be reused by any Bob user, not just our generator.

## What it does

Audits code (specifically MCP server code, but extensible) against:
- Security rules (secrets, banned libs, auth)
- GDPR (consent, residency, erasure)
- SOC2 (access controls, monitoring)
- HIPAA (PHI handling, encryption, audit logs)

## Rule schema

```json
{
  "id": "no-hardcoded-secrets",
  "category": "security",
  "severity": "high",
  "frameworks": ["SOC2", "HIPAA"],
  "description": "Detects hardcoded API keys, passwords, tokens",
  "pattern": {
    "type": "regex",
    "expression": "(api[_-]?key|password|token)\\s*=\\s*['\\\"][^'\\\"]+['\\\"]"
  },
  "auto_fix": {
    "type": "env_var",
    "template": "process.env.{NAME}"
  }
}
```

## Day 1 goal
- MCP server skeleton with `audit` tool
- 5 starter rules: hardcoded-secrets, banned-libs, missing-auth, pii-in-logs, plaintext-storage
