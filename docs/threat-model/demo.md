---
title: Demo
layout: default
parent: Threat Model
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Demo Google ADK Threat Model

** Threat analysis** of  **Intelligent Travel Co. multi-agent system** based on the PyTM model.

Below is a structured analysis following the **STRIDE framework** (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege) and considering each **trust boundary** and key **dataflow**.

---

# 🧠 Threat Model Analysis: Intelligent Travel Co. (Google ADK Agents)
![Demo Google ADK Threat Model](/assets/images/demo-google-adk-threat-model.png)

---

## 🔹 1. System Overview

The system is a **multi-agent orchestration platform** deployed on **Google Cloud**, using **ADK Agents** for trip planning and booking.
Data and actions flow between:

* End-users (travelers),
* Google Cloud-hosted agents (TripCoordinator, FlightAgent, HotelAgent, PolicyAgent),
* Internal tools (Cloud Function, AlloyDB, GCS),
* External APIs (Google Calendar, Stripe, flight aggregators).

Security mechanisms include:

* TLS for all traffic,
* Google IAM service accounts (least privilege),
* OAuth 2.0 for user consent,
* VPC Service Controls (isolation),
* Model Armor (prompt & PII inspection),
* PolicyAgent (guardrails).

---

## 🔹 2. Trust Boundary Summary and Risk Surface

| **Boundary**                               | **Description**                                                                      | **Main Threats**                                                                            | **Mitigations**                                                              |
| ------------------------------------------ | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **User ↔ Cloud (Edge)**                    | End-user browser/mobile interacting with cloud-hosted Model Armor & TripCoordinator. | Spoofing (fake user), Injection (malicious prompt), DoS (traffic flood), Man-in-the-Middle. | TLS, authentication, Model Armor scanning, rate limiting.                    |
| **Cloud ↔ External APIs**                  | Agents invoking Stripe, Google Calendar, external aggregators.                       | API key leakage, token theft, data exfiltration, replay attacks.                            | Secret Manager, short-lived OAuth tokens, least-privilege keys, secure SDKs. |
| **VPC Perimeter (Internal Cloud Network)** | Agents, AlloyDB, GCS, IAM, and internal Cloud Functions.                             | Privilege escalation, lateral movement, data tampering, internal exfiltration.              | VPC-SC isolation, IAM role scoping, service-to-service auth, monitoring.     |
| **Agent ↔ Internal Tool (Cloud Function)** | FlightAgent invoking pricing Cloud Function (Lambda).                                | Injection into query payloads, privilege misuse, abuse of tool interface.                   | Input sanitization, read-only service account, parameterized queries.        |

---

## 🔹 3. STRIDE Analysis by Dataflow

### 1️⃣ End-User → Model Armor → TripCoordinator

**Data:** Natural-language prompt, possibly containing user data.
**Crosses:** User ↔ Cloud boundary.

| STRIDE Category | Threat                                         | Impact                                  | Mitigation                                        |
| --------------- | ---------------------------------------------- | --------------------------------------- | ------------------------------------------------- |
| **S**           | Spoofing of end-user (unauthenticated request) | Unauthorized access or malicious prompt | Require OAuth login, CAPTCHA, or token validation |
| **T**           | Tampering (prompt injection)                   | Model or agent misbehavior              | Model Armor input sanitization                    |
| **I**           | Information Disclosure                         | Leaked PII in prompt                    | Model Armor redaction                             |
| **D**           | Denial of Service                              | Overload via large/complex prompts      | Rate limits, input length checks                  |

---

### 2️⃣ TripCoordinator ↔ FlightAgent ↔ PolicyAgent

**Data:** Structured JSON describing booking request.
**Internal flow (same VPC)**

| STRIDE | Threat                                               | Impact                                             | Mitigation                                         |
| ------ | ---------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------- |
| **S**  | Spoofed A2A messages (unverified sub-agent identity) | Unauthorized agent commands                        | mTLS between agents or ADK agent authentication    |
| **T**  | Tampering with policy approval messages              | Policy bypass (e.g., approving disallowed booking) | Signed internal messages, logs                     |
| **R**  | Lack of logging                                      | No audit trail of policy enforcement               | Centralized audit logging (Stackdriver, Chronicle) |

---

### 3️⃣ FlightAgent → Internal Pricing Tool → AlloyDB

**Data:** Query for partner discounts.
**Crosses:** Agent ↔ Tool boundary.

| STRIDE | Threat                              | Impact                            | Mitigation                                     |
| ------ | ----------------------------------- | --------------------------------- | ---------------------------------------------- |
| **T**  | SQL Injection into function payload | Read/Write data corruption        | Input validation, parameterized queries        |
| **E**  | Misuse of Cloud Function privileges | Lateral access to other resources | Separate service account with read-only access |
| **I**  | Database leakage                    | Discount or user data exposure    | TLS, IAM restrictions, limited query scope     |

---

### 4️⃣ FlightAgent → Google Calendar API (OAuth, user-auth)

**Data:** OAuth tokens, user event data.
**Crosses:** Cloud ↔ External API boundary.

| STRIDE | Threat                              | Impact                          | Mitigation                                    |
| ------ | ----------------------------------- | ------------------------------- | --------------------------------------------- |
| **S**  | Spoofing of Calendar API endpoint   | Credential theft                | Use HTTPS + certificate pinning               |
| **I**  | Token leakage                       | Unauthorized calendar access    | Store tokens in Secret Manager or memory only |
| **R**  | Missing audit trail of user consent | Legal and privacy noncompliance | Log OAuth grant & revoke events               |

---

### 5️⃣ FlightAgent → Stripe Payment API

**Data:** Payment token, amount, booking metadata.
**Crosses:** Cloud ↔ External API boundary.

| STRIDE | Threat                               | Impact            | Mitigation                                 |
| ------ | ------------------------------------ | ----------------- | ------------------------------------------ |
| **S**  | Impersonation (fake Stripe endpoint) | Payment diversion | HTTPS validation, trusted SDK              |
| **I**  | Token or PII disclosure              | Data breach       | Never log sensitive fields, PCI compliance |
| **D**  | Payment API rate limit exceeded      | Booking failure   | Implement backoff & retries                |

---

### 6️⃣ FlightAgent → GCS Bucket

**Data:** Session logs, state, uploaded user files.
**Crosses:** Internal boundary.

| STRIDE | Threat                      | Impact                   | Mitigation                                 |
| ------ | --------------------------- | ------------------------ | ------------------------------------------ |
| **T**  | Tampering with session logs | Audit trail manipulation | Object immutability (GCS retention policy) |
| **I**  | PII leakage in logs         | Privacy breach           | Encrypt logs, redact sensitive data        |
| **R**  | No logging of changes       | Lack of forensic data    | Enable Cloud Audit Logs                    |

---

### 7️⃣ Developer/Admin → Google IAM

**Data:** IAM role configurations, service accounts.

| STRIDE | Threat                                    | Impact                   | Mitigation                                |
| ------ | ----------------------------------------- | ------------------------ | ----------------------------------------- |
| **E**  | Privilege escalation via misconfiguration | Compromise of all agents | Use least privilege, periodic IAM reviews |
| **R**  | Lack of role change auditing              | Insider threat           | Enable IAM audit logs                     |

---

## 🔹 4. Global System Threats

| Threat Category                         | Description                                             | Risk   | Existing Controls                  | Residual Risk                   |
| --------------------------------------- | ------------------------------------------------------- | ------ | ---------------------------------- | ------------------------------- |
| **Prompt Injection**                    | User input manipulates model or agents                  | High   | Model Armor, PolicyAgent           | Medium (novel attacks possible) |
| **Token Leakage (OAuth / API Keys)**    | Loss of secrets through logs or memory                  | High   | Secret Manager, short-lived tokens | Medium                          |
| **Data Exfiltration via External APIs** | Compromised agent sending data to unauthorized endpoint | High   | Allowlist APIs, outbound proxy     | Low                             |
| **Insider Misconfiguration (IAM)**      | Privilege escalation                                    | Medium | IAM change review, audit logs      | Low                             |
| **DoS / Abuse**                         | Flooding with heavy prompts                             | Medium | Rate limiting, Model Armor         | Low                             |
| **Sandbox Escape (Code Execution)**     | Malicious code from prompt                              | High   | Vertex AI Code Executor sandbox    | Low                             |

---

## 🔹 5. Key Strengths

✅ Clear separation of privileges using IAM & service accounts
✅ Comprehensive input validation and sanitization (Model Armor)
✅ Segregated boundaries (VPC-SC, Cloud Function segregation)
✅ Use of OAuth for user-scoped permissions (least privilege)
✅ PolicyAgent enforcing business logic & financial limits

---

## 🔹 6. Weaknesses / Gaps

⚠️ **Agent-to-Agent authentication** not explicitly defined (A2A messages could be spoofed).
⚠️ **Logging and audit trail** coverage may not include all inter-agent and policy decisions.
⚠️ **Prompt injection attacks** evolve faster than detection — residual risk remains.
⚠️ **Data minimization** for logs and user uploads not clearly enforced (possible PII leaks).
⚠️ **External API dependencies** (Stripe, aggregator) add supply-chain risk.

---

## 🔹 7. Recommended Mitigations

1. **Mutual TLS / Signed tokens** between agents for all A2A traffic.
2. **Centralized audit trail** (Stackdriver or Chronicle) for every agent decision, policy check, and payment.
3. **Periodic IAM audits** with automated least-privilege checks.
4. **Strict egress control** to prevent data exfiltration to unknown APIs.
5. **Continuous prompt injection testing** and retraining of Model Armor rules.
6. **Encrypt GCS logs at object-level**, apply retention and immutability.
7. **Privacy-aware logging** — automatically redact or hash sensitive data (names, tokens).

---

## 🧩 8. Overall Risk Posture

| Dimension           | Rating        | Comments                                                                                           |
| ------------------- | ------------- | -------------------------------------------------------------------------------------------------- |
| **Confidentiality** | 🟡 *Medium*   | Tokens, user data in transit and at rest; well-protected but risk remains at API boundary          |
| **Integrity**       | 🟢 *Low*      | PolicyAgent + IAM roles restrict destructive actions                                               |
| **Availability**    | 🟡 *Medium*   | External API dependencies (Stripe, Calendar) may cause outages                                     |
| **Accountability**  | 🟡 *Medium*   | Requires complete audit coverage                                                                   |
| **Overall Risk**    | 🟠 *Moderate* | Strong cloud-native security posture; main risks from external APIs, injection, and token handling |

---

✅ **Summary:**
Your Google ADK multi-agent architecture is *well-structured for security*, leveraging Google Cloud’s native protections (IAM, VPC-SC, Cloud Functions, OAuth). The **primary residual threats** are:

* Novel **prompt injection or data leakage**,
* **Improper IAM scoping** or misconfigurations,
* **External API compromise or misuse**.

---

## 🧩 1. What This File Is

This Python file describes your **Google ADK multi-agent travel system** in a format that the **PyTM** library understands.

PyTM (Python Threat Modeling) is a DSL (domain-specific language) for defining:

* **Elements** — people, services, servers, databases, APIs.
* **Dataflows** — how information moves between those elements.
* **Boundaries** — trust perimeters that separate elements with different privilege or identity.
* **Controls and threats** — notes and metadata that help PyTM generate a threat list, sequence diagram, or dataflow diagram.

When you run `tm.process()` at the end, PyTM:

1. Validates the model.
2. Generates diagrams (if configured).
3. Identifies likely threats using the STRIDE framework.

---

## 🏗️ 2. Major Building Blocks

### a. **Boundaries**

These define where *trust changes*.
In your system we have four:

1. **User vs Cloud (Edge)** — between the traveler’s browser/app and Google Cloud.
   ➜ Controls: TLS, authentication, Model Armor scanning.

2. **Cloud vs External APIs** — between your agents and third-party services (Stripe, Google Calendar).
   ➜ Controls: separate API keys, least-privilege access, Secret Manager.

3. **VPC Service Perimeter (Internal Network)** — your internal Google Cloud network isolating agents, functions, and databases.
   ➜ Controls: VPC-SC, IAM policies.

4. **Agent vs Tool (Internal Segregation)** — between an agent and the internal Cloud Function that queries the database.
   ➜ Controls: Cloud Function has its own read-only service account.

These correspond directly to the *trust boundaries* section in your scenario.

---

### b. **Actors**

Represent humans interacting with the system:

* `End-User (Traveler)` — the customer planning a trip.
* `Developer/Admin` — the engineer who manages the system.

---

### c. **Servers / Lambdas / Datastores**

Represent the actual *components*:

* **Servers** → long-lived compute (agents, APIs, infra like Model Armor).
* **Lambda** → short-lived, event-driven function (your internal Cloud Function).
* **DataStore** → persistent data (AlloyDB, GCS).

Each is placed *inside a boundary* to express its privilege and exposure.

---

### d. **Dataflows**

Each `Dataflow(source, destination, "description")` line defines a directional communication channel.
PyTM uses these to:

* Trace how sensitive data moves.
* Identify where data crosses boundaries.
* Generate STRIDE threats (Spoofing, Tampering, Repudiation, etc.).

For example:

```python
Dataflow(flight_agent, google_calendar_api,
         "OAuth 2.0 flow: request user's consent; then use user-scoped token")
```

tells PyTM that data (user OAuth token and flight info) flows *out of* your Cloud environment to an external API. Because this crosses from `boundary_vpc` to `boundary_cloud_external`, PyTM will mark it as a **trust boundary crossing**, prompting checks for:

* Spoofing (is the API authentic?),
* Information disclosure (are tokens stored safely?),
* Repudiation/logging, etc.

---

## 🔒 3. How the Model Encodes Security Concepts

| Concept in your prose     | How it appears in the code                           | Purpose                               |
| ------------------------- | ---------------------------------------------------- | ------------------------------------- |
| Prompt-injection scanning | `model_armor` Server + Dataflow from user through it | Input validation control              |
| Service accounts & IAM    | `google_iam` Server with notes                       | Enforces least privilege              |
| OAuth 2.0 user auth       | Dataflow with “OAuth 2.0 flow”                       | Distinguishes user-auth vs agent-auth |
| VPC Service Controls      | `boundary_vpc`                                       | Internal isolation                    |
| PolicyAgent guardrails    | Separate server + dataflow                           | Internal governance layer             |
| Cloud Function isolation  | `internal_pricing_tool` Lambda in its own boundary   | Privilege segmentation                |

---

## 🧮 4. What PyTM Does with It

Once you run:

```bash
python intelligent_travel_tm.py
```

and have `pytm` installed, you can produce:

* `--dfd` → a **Data Flow Diagram** showing all the elements and trust boundaries.
* `--seq` → a **Sequence Diagram** for request flow.
* `--report` → a **Threat Report** listing potential STRIDE threats with mitigation hints.

Example:

```bash
python intelligent_travel_tm.py --dfd
python intelligent_travel_tm.py --report
```

You’ll see something like:

```
[THREAT] T01: Spoofing of external API (Cloud vs Third-Party boundary)
[THREAT] T07: Data exfiltration via OAuth token
[THREAT] T12: SQL injection into Internal Pricing Tool
...
```

---

## 💡 5. Why This Matters

This modeling approach lets you:

1. **Visualize** all agent interactions across Google Cloud and third-party APIs.
2. **Quantify risk** where trust boundaries are crossed (e.g., user ↔ Model Armor ↔ TripCoordinator).
3. **Systematically check** mitigations (TLS, IAM, VPC-SC, service accounts, sandboxed code).
4. **Automate threat detection** — PyTM uses STRIDE rules to identify typical threats.

---

```python
#!/usr/bin/env python3

# tm.py - Intelligent Travel Co. threat model example
from pytm import TM, Boundary, Actor, Server, Dataflow, Datastore, Lambda

tm = TM("Intelligent Travel Co. - Google ADK Agents")

# -------------------------
# Boundaries / Trust perimeters
# -------------------------
boundary_user_cloud = Boundary("User vs Cloud (Edge)")
boundary_cloud_external = Boundary("Cloud vs Third-Party APIs")
boundary_vpc = Boundary("VPC Service Perimeter (Internal Network)")
boundary_internal_tooling = Boundary("Agent vs Tool (Internal Segregation)")

# -------------------------
# Actors
# -------------------------
end_user = Actor("End-User (Traveler)")
end_user.inBoundary = boundary_user_cloud

developer = Actor("Developer/Admin")
# Developer/Admin manages cloud but typically interacts from outside; place them in cloud boundary for model purposes
developer.inBoundary = boundary_vpc

# -------------------------
# Cloud / Agent elements (hosted inside VPC perimeter)
# -------------------------
# Root & sub agents hosted on Cloud Run / Vertex AI Agent Engine
trip_coordinator = Server("TripCoordinator (Root Agent)")
trip_coordinator.description = "ADK Root agent that receives user goal and orchestrates sub-agents"
trip_coordinator.inBoundary = boundary_vpc

flight_agent = Server("FlightAgent (Sub-Agent)")
flight_agent.description = "Searches/books flights; performs OAuth user-scoped API calls when needed"
flight_agent.inBoundary = boundary_vpc

hotel_agent = Server("HotelAgent (Sub-Agent)")
hotel_agent.description = "Searches/Books accommodations"
hotel_agent.inBoundary = boundary_vpc

policy_agent = Server("PolicyAgent (Guardrail Agent)")
policy_agent.description = "Checks requests/responses against company policy (approve/deny)"
policy_agent.inBoundary = boundary_vpc

# Infrastructure (represent as Servers / Services inside same VPC perimeter)
model_armor = Server("Model Armor (Edge Scanner)")
model_armor.description = "Scans incoming prompts/responses for prompt injection, PII, policy violations"
model_armor.inBoundary = boundary_vpc

cloud_run = Server("Google Cloud Run (host for agents)")
cloud_run.inBoundary = boundary_vpc

vertex_ai_engine = Server("Vertex AI Agent Engine")
vertex_ai_engine.inBoundary = boundary_vpc

google_iam = Server("Google Cloud IAM")
google_iam.description = "Service accounts and IAM policies"
google_iam.inBoundary = boundary_vpc

# -------------------------
# Internal Tooling & Data stores
# -------------------------
internal_pricing_tool = Lambda("Internal Pricing Tool (Cloud Function)")
internal_pricing_tool.description = "Cloud Function used by agents to fetch partner discounts"
internal_pricing_tool.inBoundary = boundary_internal_tooling
# Note: the cloud function is logically inside the VPC perimeter, but we keep a
# distinct boundary for 'Agent vs Tool' and therefore do not overwrite the
# lambda's `inBoundary` value.

alloydb = Datastore("AlloyDB Database")
alloydb.description = "User profiles, booking history, partner discount tables"
alloydb.inBoundary = boundary_vpc

gcs_bucket = Datastore("GCS Bucket (Session logs, uploads)")
gcs_bucket.description = "Persistent session history, conversation logs, uploaded documents"
gcs_bucket.inBoundary = boundary_vpc

# -------------------------
# External Third-Party APIs (outside Cloud)
# -------------------------
google_calendar_api = Server("Google Calendar API")
google_calendar_api.inBoundary = boundary_cloud_external

stripe_api = Server("Stripe Payment API")
stripe_api.inBoundary = boundary_cloud_external

external_flight_aggregator = Server("External Flight Aggregator API")
external_flight_aggregator.inBoundary = boundary_cloud_external

# -------------------------
# Dataflows (interactions)
# -------------------------
# 1. End-User -> Edge scanner (Model Armor) -> TripCoordinator
Dataflow(end_user, model_armor, "User request (text/audio) — TLS")
Dataflow(model_armor, trip_coordinator, "Sanitized request after prompt-injection & PII checks")

# 2. TripCoordinator -> FlightAgent (A2A delegation)
Dataflow(trip_coordinator, flight_agent, "A2A: 'Book flight to Paris next Tue' (delegation + context)")

# 3. FlightAgent -> PolicyAgent (guardrail check)
Dataflow(flight_agent, policy_agent, "Policy check: booking constraints (e.g., cost < $5,000)")

# 4. FlightAgent -> Internal Pricing Tool (Cloud Function)
Dataflow(flight_agent, internal_pricing_tool, "Invoke pricing tool to query partner discounts (service-account auth)")

# 5. Internal Pricing Tool -> AlloyDB
Dataflow(internal_pricing_tool, alloydb, "Query partner discount tables (read-only)")

# 6. FlightAgent -> Google Calendar API (OAuth / user-authorized)
Dataflow(flight_agent, google_calendar_api,
         "OAuth 2.0 flow: request user's consent; then use user-scoped token to read calendar (user-auth)")

# 7. FlightAgent -> External Flight Aggregator (agent-auth / API key)
Dataflow(flight_agent, external_flight_aggregator, "Search flights (agent service API key)")

# 8. FlightAgent & HotelAgent -> Stripe (payments)
Dataflow(flight_agent, stripe_api, "Process payment (tokenized payment method via Stripe; agent uses own API key)")
Dataflow(hotel_agent, stripe_api, "Process payment (agent uses own API key)")

# 9. FlightAgent -> GCS (session logs)
Dataflow(flight_agent, gcs_bucket, "Save search results & session state (logs, user uploads)")

# 10. TripCoordinator -> End-User (stream results)
Dataflow(trip_coordinator, end_user, "Stream flight options / booking confirmation to user (TLS)")

# 11. Developer/Admin -> Google IAM (manage service accounts & roles)
Dataflow(developer, google_iam, "Manage service accounts, IAM roles, and key rotation")

# 12. Model Armor <-> PolicyAgent (optional internal telemetry)
Dataflow(model_armor, policy_agent, "Telemetry: flagged prompts & policy rulings (for audit)")

# -------------------------
# Notes / Controls (attributes on elements; helpful for analysis)
# -------------------------
# Example: indicate least-privilege service account use
google_iam.notes = (
    "Service accounts used:\n"
    "- Agent service accounts (least-privilege; Cloud Function Invoker only for pricing tool)\n"
    "- Separate API keys for third-party APIs stored in Secret Manager\n"
)

model_armor.notes = "Enforces prompt injection detection, PII redaction, rate-limit/DoS protections at ingress."

internal_pricing_tool.notes = (
    "Runs with a dedicated service account with read-only access to AlloyDB. "
    "Sandboxed execution; validates and sanitizes inputs to prevent SQL injection."
)

# Mark important trust boundaries in the TM description (human-readable)
boundary_user_cloud.notes = "Edge boundary: MITM, prompt injection, unauthenticated requests. Controls: TLS, auth, Model Armor."
boundary_cloud_external.notes = "Agent -> Third-party external APIs. Controls: Secret Manager, per-service API keys, rate limiting."
boundary_vpc.notes = "VPC Service Controls enforce that only specific Cloud Run services can access AlloyDB and GCS."
boundary_internal_tooling.notes = "Cloud Function runs as separate service account; limited privileges to reduce blast radius."

# End of model
if __name__ == "__main__":
    tm.process()
```