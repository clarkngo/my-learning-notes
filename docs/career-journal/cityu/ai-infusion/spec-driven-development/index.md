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

Spec-Driven Development (SDD) is a workflow where a formal specification—requirements, API contracts, data schemas, or acceptance criteria—serves as the primary input that drives code generation and validation. Pairing SDD with an AI coding platform turns the spec into an executable contract: the AI generates implementation skeletons, tests, and documentation directly from the spec, and later validates that code changes remain spec-compliant.

---

## Core Principles

1. **Spec is the source of truth.** Code is derived from the spec, not the other way around.
2. **AI amplifies, not replaces, the spec author.** Humans own the requirements; AI handles the mechanical translation into code artifacts.
3. **Continuous validation.** Every change is checked against the spec automatically, not just at review time.

---

## Workflow

### Step 1: Write the Spec First

Before any code is written, produce a machine-readable or structured natural-language specification:

- **API specs:** OpenAPI / AsyncAPI YAML
- **Data specs:** JSON Schema, Protobuf, or ERD
- **Feature specs:** Gherkin (Given/When/Then) or structured user stories with acceptance criteria
- **Architecture specs:** ADRs (Architecture Decision Records) with defined constraints

### Step 2: Generate Implementation Artifacts from the Spec

Feed the spec into an AI coding platform (e.g., Claude, GitHub Copilot with context, Cursor) with a generation prompt.

**Example — API Spec to Controller:**
> "Given the following OpenAPI spec, generate a TypeScript Express controller with input validation, error handling, and JSDoc comments. Do not add logic beyond what the spec defines."

**Example — Gherkin to Test Suite:**
> "Convert the following Gherkin scenarios into Jest test cases. Use descriptive `it()` blocks that map 1:1 to each scenario step."

### Step 3: AI-Assisted Spec Review

Before implementation begins, use the AI to stress-test the spec itself.

**Example Prompt:**
> "Review this API spec for ambiguities, missing error codes, and edge cases not covered by the defined paths. List each gap with a suggested resolution."

### Step 4: Continuous Spec Compliance Checks

Integrate AI-assisted compliance checks into the CI pipeline or as a pre-commit hook:

- Diff the spec against the implementation to detect drift.
- Ask the AI to verify that a proposed code change is consistent with the spec.

**Example Prompt:**
> "Here is the current OpenAPI spec and the proposed code change below. Does the change introduce any behavior that is not described in the spec, or break any existing contract? Explain each discrepancy."

### Step 5: Auto-Generate Documentation from Spec + Code

Once code is implemented, use AI to reconcile the spec and implementation into developer-facing documentation that reflects what was actually built.

---

## Toolchain Example

| Layer | Tool | AI Role |
| --- | --- | --- |
| **Spec authoring** | OpenAPI Editor, Notion, Confluence | Suggest missing fields, validate structure |
| **Code generation** | Claude Code, Cursor, Copilot | Generate controllers, models, tests from spec |
| **Spec compliance** | Custom CI step + AI prompt | Flag implementation drift from spec |
| **Documentation** | AI + spec + code | Auto-generate README, API reference |

---

## Benefits

- **Reduced ambiguity:** Forcing a written spec before coding surfaces unclear requirements early.
- **Faster scaffolding:** AI generates boilerplate from the spec in seconds, letting developers focus on business logic.
- **Traceability:** Every generated artifact is traceable back to a spec section.
- **Onboarding:** New team members can read the spec and ask the AI to explain the corresponding code, reducing ramp-up time.

---

## Pitfalls to Avoid

| Pitfall | Mitigation |
| --- | --- |
| Over-relying on AI-generated code without review | Treat generated code as a draft; always review for correctness |
| Specs that are too vague for generation | Enforce a spec review gate before generation begins |
| Spec drift after initial generation | Automate compliance checks in CI so drift is caught immediately |
| AI hallucinating spec details | Ground prompts with the actual spec text; do not ask AI to recall it from memory |

---

## Implementation Note

Introduce SDD incrementally: start with one API endpoint or one feature module. Define the spec, generate the skeleton with AI, review and refine, then expand the practice to the rest of the project. Teams new to spec-driven workflows benefit from seeing the AI output as a concrete example of what a good spec enables before writing larger specs themselves.
