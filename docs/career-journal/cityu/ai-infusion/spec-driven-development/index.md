---
title: Spec-Driven Development with AI
layout: default
parent: AI Infusion
---


# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


# Spec-Driven Development with AI Coding Platform

## Overview

Spec-Driven Development (SDD) is a workflow where a formal specification—requirements, API contracts, data schemas, or acceptance criteria—serves as the primary input that drives code generation and validation. This approach is demonstrated in **HOS10A: AI-Powered Web Application** (CS628 Full-Stack Development), where students derive all implementation decisions from a simulated client conversation before writing a single line of code.

---

## The HOS10A Architecture

HOS10A builds a RAG-powered employee records application on a four-layer stack. The entire architecture is derived from a single client conversation thread before any implementation begins.

```
React (Frontend, port 3000)
    │  POST /chat  (SSE stream)
    ▼
Node/Express (Backend, port 5050)
    │  $text search (limit 5)        POST to Ollama API
    ▼                                     ▼
MongoDB Atlas (records collection) → Gemma 2:2b (Ollama, port 11434)
    text index on: name, position
```

**RAG Flow:**
```
User query → MongoDB $text search → top 5 results as context
    → augmented prompt → Gemma 2:2b → SSE stream → React UI
```

---

## Section 1: Spec-Driven Development Workflow

### Step 1.1 — The Client Conversation (Source of Truth)

All architecture, API routes, streaming design, MongoDB index requirements, and screenshot deliverables are embedded in a single Slack-style conversation thread between three personas:

| Persona | Role | Contribution |
| --- | --- | --- |
| Maria Chen | Product Manager | Defines the feature goal, stack, and RAG flow |
| Alex Rivera | Developer | Asks clarifying questions (e.g., how SSE differs from regular API calls) |
| Sam Park | Tech Lead | Specifies exact implementation: route names, SSE headers, MongoDB query, prompt construction, streaming terminator |

The conversation is the spec. Nothing is built before it is read and understood.

### Step 1.2 — Requirements Gathering with AI

Use Claude or Gemini to formally extract requirements from the conversation. Three structured prompts drive this phase:

**Exercise 1.2.1 — Actors & Goals**
> "From this scenario, identify all system actors (users, services, external systems) and their primary goals. Format as a table: Actor | Type | Primary Goal. Then paste the Section 1 conversation."

**Exercise 1.2.2 — Functional Requirements**
> "Extract all functional requirements. Format each as: 'The system shall...' Organize by component."

Expected requirements extracted from the HOS10A conversation:

- The system shall run Ollama with Gemma 2:2b locally on port 11434.
- The system shall add a `POST /chat` route to `record.mjs` implementing the RAG pipeline.
- The system shall query MongoDB with `$text` search to retrieve up to 5 relevant records as context.
- The system shall construct an augmented prompt combining retrieved context and user message.
- The system shall stream the Gemma 2 response to the frontend via Server-Sent Events (SSE).
- The system shall send a `[DONE]` SSE marker when streaming is complete.
- The system shall create a `chat.js` frontend component that parses the SSE response stream.
- The system shall configure a MongoDB text index on `name` and `position` fields.
- The system shall store the backend URL in the frontend `.env` as `REACT_APP_BACKEND_URL`.

**Exercise 1.2.3 — Non-Functional Requirements**
> "Extract non-functional requirements: performance, security, privacy, maintainability. Describe each constraint and why it matters."

**Exercise 1.2.4 — Open Questions**
> "What are at least 5 unresolved engineering questions not answered in the conversation?"

Write at least 3 original open questions (not AI-generated) in DESIGN_DOC.md Section 11.

### Step 1.3 — The Design Document (Gate Before Coding)

> Do NOT start coding until your `DESIGN_DOC.md` is complete and reviewed.

Create the file at the project root: `touch DESIGN_DOC.md`

Generate each section with AI using these prompt templates:

| Section | AI Prompt |
| --- | --- |
| 1. Problem Statement | Write a 2-sentence problem statement for this system. Avoid buzzwords. |
| 2. Goals | List 4-6 concrete measurable goals based on the conversation. |
| 3. Non-Goals | List 3-4 explicit non-goals — things deliberately NOT built this sprint. |
| 4. Actors | Describe all actors and their roles. |
| 5. Functional Requirements | Generate 'The system shall...' statements organized by component. |
| 6. Non-Functional Requirements | List performance, security, privacy, and maintainability constraints. |
| 7. Architecture Overview | Draw an ASCII text diagram of the full system data flow. |
| 8. API Design | Document all API endpoints: path, method, request body, response, status codes. |
| 9. Data Model | Describe the data structures and schemas flowing through the system. |
| 10. Tech Stack Justification | For each technology, write 1-2 sentences explaining WHY it was chosen. |
| 11. Open Questions | List at least 5 unresolved questions including your 3 original ones. |

**Hallucination Validation Prompt:**
> "Review this DESIGN_DOC.md for technical accuracy. Flag incorrect port numbers, wrong API paths, missing configurations, or misnamed technologies. Cross-reference: [paste Section 1 conversation]."

---

## AI Coding Assistant Workflows

With `DESIGN_DOC.md` as the primary context source, choose a platform:

### Option A: Cursor (AI-Native IDE)
1. Open the project in Cursor.
2. Open the AI Chat panel.
3. Add context: type `@DESIGN_DOC.md` in the chat input.
4. Paste each milestone prompt to generate code step by step.

### Option B: VS Code + GitHub Copilot
1. Open Chat view (`Cmd+Shift+I`).
2. Type `@workspace` at the start of each prompt so Copilot indexes all project files including `DESIGN_DOC.md`.
3. Provide milestone instructions to generate or modify files inline.

---

## Implementation Milestones

### Milestone 1 — Ollama + Gemma 2 Setup

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama serve
# new terminal:
ollama pull gemma2:2b
ollama run gemma2:2b
```

Confirm port 11434 is accessible in the Codespaces Ports tab.

### Milestone 2 — Backend `/chat` RAG Route

```bash
cd backend
npm install axios
```

The `POST /chat` handler in `record.mjs` must:

1. Read `req.body.content` as the user message.
2. Set SSE headers: `Content-Type: text/event-stream`, `Cache-Control: no-cache`, `Connection: keep-alive`.
3. Query MongoDB: `db.collection('records').find({ $text: { $search: message } }).limit(5)`.
4. Format top results into a context string with `name`, `position`, `level` fields.
5. Construct augmented prompt: `Context + User Message + 'Bot:'`.
6. POST to `http://localhost:11434/api/generate` with `model: gemma2:2b` and `responseType: stream` via axios.
7. Stream each chunk back as `data: <chunk>`. Send `data: [DONE]` at end.

### Milestone 3 — MongoDB Text Index

In MongoDB Atlas: Data Explorer → records collection → Indexes → Create Index on `name` (text) and `position` (text). Required for `$text` search to work.

### Milestone 4 — Frontend `chat.js` + `.env`

Create `.env` in `frontend/`:
```
REACT_APP_BACKEND_URL=<your port 5050 forwarded URL>
```

`chat.js` in `src/components/` must POST to `REACT_APP_BACKEND_URL/chat`, parse SSE stream chunks, and display tokens progressively.

Update `App.js`: `/` → Chat, `/record` → recordList.

### Milestone 5 — 3 RAG Test Scenarios

| Test | Action | Expected Result |
| --- | --- | --- |
| 1. Existing record | Query an employee name that exists in MongoDB | AI responds with their details from context |
| 2. Non-existing record | Query a name not in the database | AI responds that no data was found |
| 3. New record | Create a record via `/record`, then query via chat | AI retrieves and describes the newly created record |

---

## Key Design Decisions from the Spec

| Decision | Spec Source | Reason |
| --- | --- | --- |
| SSE over WebSockets | Sam Park's message | One-way server-to-client streaming; simpler for this use case |
| `$text` search over vector search | Maria Chen's architecture spec | MongoDB native; sufficient for name/title/level lookups |
| `gemma2:2b` | Maria Chen's stack definition | Lightweight local model; no external API dependency |
| `axios` with `responseType: stream` | Sam Park's implementation detail | Handles chunked streaming from Ollama to Express |
| `[DONE]` sentinel | Sam Park's SSE protocol spec | Frontend knows when to stop parsing the stream |

---

## Broader SDD Principles Demonstrated

HOS10A shows that spec-driven development with AI is most effective when:

1. **The spec is a real conversation**, not an abstract document — ambiguity surfaces naturally when personas with different roles discuss the system.
2. **AI extracts, humans verify** — AI formats requirements and flags gaps, but the developer confirms correctness against the original conversation.
3. **The design doc gates implementation** — no milestone begins until `DESIGN_DOC.md` is validated, preventing the most common cause of rework.
4. **AI coding assistants use the spec as context** — `@DESIGN_DOC.md` in Cursor or `@workspace` in Copilot grounds generation in the actual system design rather than generic patterns.
5. **Hallucination validation is a required step** — the spec is cross-referenced against the AI-generated doc before any code is written.
