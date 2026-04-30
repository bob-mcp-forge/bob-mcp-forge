# Day 1 Quickstart — Per-Person Checklist

## 🧑‍💻 Anthony (Architecture + Compliance Engine)
- [ ] Set up GitHub org + invite everyone
- [ ] Create starter repo with folder structure
- [ ] Draft compliance engine MCP server skeleton
- [ ] Define rule schema (JSON format)
- [ ] Write 5 starter rules: hardcoded-secrets, banned-libs, missing-auth, pii-in-logs, plaintext-storage
- [ ] Write demo script v1

## 🤖 Alejandro (MCP Generator + Agentic)
- [ ] Set up `backend/generator/` workspace
- [ ] Hardcode a sample MCP server template (we'll plug Bob in Day 2)
- [ ] Define generator I/O contract (see ARCHITECTURE.md)
- [ ] Build a simple test: input = "search emails", output = MCP server files
- [ ] Document the generator API for the team

## ⚙️ Dilshad (Backend + DevOps)
- [ ] Set up `backend/api/` with Express
- [ ] Build `/api/generate` endpoint (mocked output for now)
- [ ] Spin up MongoDB Atlas free tier OR local SQLite
- [ ] Sign up for IBM Cloud free tier
- [ ] Deploy a "hello world" container to Code Engine to validate the pipeline

## 🎨 Tien (Frontend)
- [ ] Set up `frontend/` with Vite + React + Tailwind
- [ ] Build the input form screen
- [ ] Build the report screen (use mock data from ARCHITECTURE.md)
- [ ] Wire up routing
- [ ] Pick a color palette + basic components (or use shadcn/ui)

## 📋 Sadia (Design + Docs + Pitch)
- [ ] Wireframe all 4 screens (Figma or pen & paper, just get them down)
- [ ] Drop wireframes in `docs/wireframes/`
- [ ] Start pitch deck outline (10 slides max)
- [ ] Write the elevator pitch (3 sentences)
- [ ] Once compliance rules are drafted, help with copy for the report UI

---

## Coordination Points

**End of Day 1 sync (suggested 8pm ET):**
- 15-min Google Meet
- Each person shows what they have
- Identify any blockers
- Confirm Day 2 plan

**Day 2 milestones:**
- Morning: integration begins (generator + compliance + frontend wired)
- Afternoon: real Bob calls replacing mocks
- Evening: end-to-end demo on localhost

**Day 3 deadline:**
- Submit by [TIME TBD — confirm with hackathon site]
- Allow 2 hours buffer for video upload + final deck tweaks

---

## When You're Stuck

1. Drop the question in the LinkedIn chat (or Discord once it's up)
2. Tag the relevant person
3. If still stuck after 30 min, hop on a Google Meet
4. Don't burn 4 hours on something — ask early

We're a team. Help is faster than struggle.
