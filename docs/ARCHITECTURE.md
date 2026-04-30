# Architecture Deep Dive

## System Components

### 1. Frontend (React + Tailwind)
**Owner:** Tien
**Responsibilities:**
- Plain-English input form
- Real-time generation status
- Compliance report visualization (pass/warn/fail per rule)
- Auto-fix preview
- Deploy button + live URL display

**Key screens:**
- `/` — Landing + input
- `/generate/:id` — Live generation + streaming code preview
- `/review/:id` — Compliance report with inline fixes
- `/deployed/:id` — Success state with live URL + how to connect to Bob

### 2. Generator Service
**Owner:** Alejandro
**Responsibilities:**
- Take natural language description
- Query Bob to produce MCP server boilerplate
- Output: tool schema, handler functions, auth config, package.json
- Validate generated code parses/compiles before passing forward

**Inputs:** `{ description: string, context?: object }`
**Outputs:** `{ files: { path: string, content: string }[], schema: object }`

### 3. Compliance Agent (MCP Server)
**Owner:** Anthony
**This is our differentiator. Built AS an MCP server so we eat our own dog food.**

**Rule categories:**
- **Secrets & credentials:** hardcoded API keys, passwords, tokens
- **Banned libraries:** outdated/insecure packages, license incompatibilities
- **Data handling:** PII detection, encryption-in-transit checks
- **Auth patterns:** missing auth, weak auth, role checks
- **Logging:** PII leakage in logs, audit trail completeness
- **GDPR:** consent flows, data residency, right-to-erasure
- **SOC2:** access controls, monitoring hooks
- **HIPAA:** PHI handling, audit logs, encryption at rest

**Output format:**
```json
{
  "score": 78,
  "results": [
    {
      "rule": "no-hardcoded-secrets",
      "category": "security",
      "severity": "high",
      "status": "fail",
      "location": { "file": "handler.ts", "line": 23 },
      "message": "Hardcoded API key detected",
      "auto_fix": { "type": "env_var", "suggestion": "..." }
    }
  ]
}
```

### 4. Orchestrator API
**Owner:** Dilshad
**Responsibilities:**
- Single entry point for the frontend
- Orchestrate: generator → compliance agent → deploy
- Persist generation history (MongoDB or SQLite)
- Stream progress events back to frontend (SSE or WebSocket)

### 5. Deploy Service
**Owner:** Dilshad (with Anthony assist)
**Responsibilities:**
- Package generated code as Docker container
- Push to IBM Cloud Code Engine
- Return public URL + connection instructions

---

## Data Flow

```
User → Frontend
  → POST /api/generate { description }
    → Orchestrator
      → Generator.generate(description) [calls Bob]
      → ComplianceAgent.audit(files)  [MCP call to our policy server]
      → returns combined result
    ← Response: { files, complianceReport }
  ← Frontend renders report

User clicks "Apply fixes"
  → Frontend: PATCH /api/generation/:id/fixes
    → Orchestrator re-runs compliance check
    ← Updated report

User clicks "Deploy"
  → Frontend: POST /api/generation/:id/deploy
    → Orchestrator → DeployService.deploy(files)
      → Docker build → IBM Cloud Code Engine push
    ← { url: "https://...mcp.ibm.cloud" }
  ← Success screen
```

---

## Why Compliance Engine = MCP Server (not just a library)

Two reasons:
1. **It's a flex.** We're showing IBM that MCP can be used for *meta-tooling*, not just data access.
2. **It's reusable.** Other Bob users could connect to our compliance server independently to audit any code, not just generated code.

This is the angle that lifts us from "cool generator" to "platform play."

---

## Stretch Goals (if Day 2 is on track)

- **Multiple compliance profiles:** user picks "Healthcare" → loads HIPAA profile only
- **Compliance rule editor:** companies define their own rules in YAML
- **Diff view:** before/after when applying fixes
- **Marketplace:** browse other generated MCP servers
