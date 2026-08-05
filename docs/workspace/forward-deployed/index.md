---
title: Forward Deployed Game
layout: default
parent: Workspace
---

That is a grounded, mechanically tight foundation for an educational, narrative-heavy RPG. Grounding the stats in real-world FDE / Enterprise AI skills—where your stat level acts as an **intellectual lens** unlocking deeper questions—creates a compelling learning loop.

Here is a full breakdown of how to structure this **Progression & Question-Gating System** so it feels authentic to *Disco Elysium* while staying fun and educational.

---

## 1. The Question-Gating System (Tiered Inquiries)

In standard RPGs, stats determine *if* you pass a dice roll. In your system, stats determine **what level of question your character is smart enough to formulate**.

As stats rise, the player shifts from asking superficial questions to asking high-leverage architectural questions.

```
┌─────────────────────────────────────────────────────────────┐
│                 QUESTION TIER STRUCTURE                     │
├─────────────────────────────────────────────────────────────┤
│ TIER 0: BASIC (Stat Level 0-1)                              │
│ • Surface-level observations & basic corporate pleasantries. │
│ • Example: "What software do you guys use here?"            │
├─────────────────────────────────────────────────────────────┤
│ TIER 1: INTERMEDIATE (Stat Level 2-3)                       │
│ • Identifies functional pain points and manual workarounds. │
│ • Example: "How do you handle rate-limiting on the ERP?"    │
├─────────────────────────────────────────────────────────────┤
│ TIER 2: ADVANCED / EXPERT (Stat Level 4+)                   │
│ • Uncovers hidden edge cases, governance risks, & true ROI. │
│ • Example: "What's the token latency overhead on the API?"  │
└─────────────────────────────────────────────────────────────┘

```

### Example Conversation Flow (Auditing an Accounting Workflow)

When the player approaches Brenda, the choices displayed depend on their current stat level:

* **[BASIC] (Always Unlocked):** *"What system do you use to process these invoices?"*
* *Result:* Brenda gives a generic, defensive answer: *"We use the main company portal."* (Gives 0 actionable insight).


* **[INTERMEDIATE] (Requires `SHADOW AUDIT` Level 2):** *"I see a custom spreadsheet on your secondary monitor. Do you use that when the mainframe queues back up?"*
* *Result:* Brenda admits to the manual bypass. Unlocks a clear understanding of the real workflow.


* **[ADVANCED] (Requires `LEGACY WHISPERER` Level 4 or `ROI RHETORIC` Level 4):** *"If we pipe an LLM parser into that spreadsheet's batch queue, what’s your actual error rate on line-item mismatches?"*
* *Result:* Brenda is impressed. She reveals exact operational metrics and grants access to raw dataset samples.



---

## 2. Real-World Stat Growth Systems (How Players Earn Stats)

Since the player starts at **0 in every stat**, progression should mirror real-world professional development in Forward Deployed Engineering:

```
                          EARNING STATS
                                │
   ┌────────────────────┬───────┴────────┬────────────────────┐
   ▼                    ▼                ▼                    ▼
READING / STUDY      ON-SITE AUDITS    SUCCESSFUL ROLLS   FAILED ROLLS
 (Technical Docs)    (Interactions)   (Field Testing)   (Lessons Learned)

```

### Method A: Technical Reading & Inspection (The "Thought Cabinet" Lite)

* **Inspecting the Environment:** Examining server racks, reading employee sticky notes, or reading ancient API documentation lying on a desk awards specific stat points.
* *Example:* Reading an unpatched DB log file awards **+1 LEGACY WHISPERER**.
* *Example:* Reading the company's annual financial report awards **+1 ROI RHETORIC**.



### Method B: Observation & Active Listening

* Selecting basic dialogue choices and observing NPC behavior grants progress toward higher stat tiers.
* Asking 3 basic questions about employee frustration unlocks **+1 EMPATHY ENGINE**.



### Method C: Learning from Failure (Disco-Style Experience)

* Failing a low-level check shouldn't just be a wall—it should teach a lesson.
* *Example:* If you attempt an advanced prompt optimization question and fail miserably, your character realizes their mistake, earning **+1 PROMPT SYNTAX** for next time.



---

## 3. Educational Design (Learning Real-World FDE Concepts)

To ensure the player actually learns real software engineering and business consulting concepts, every stat represents a real skill:

| Stat Name | Real-World Concept Taught | Game Narrative Benefit |
| --- | --- | --- |
| **`LEGACY WHISPERER`** | Technical Debt, Monoliths, REST/SOAP APIs | Uncovers backend system constraints & system architecture. |
| **`HUMAN-IN-THE-LOOP`** | UX Design, Guardrails, Exception Handling | Designs safe AI interfaces that workers won't reject. |
| **`SHADOW AUDIT`** | Business Process Mapping, Workflow Analysis | Finds how work *actually* gets done vs. official documentation. |
| **`ROI RHETORIC`** | Financial Modeling, OPEX/CAPEX, Token Costs | Convinces executives and secures project funding. |
| **`PROCUREMENT ARMOR`** | SOC2 Compliance, Data Privacy, Data Governance | Prevents security audits and legal blockades. |

---

## 4. How to Structure This in Godot / Dialogic

In Godot, you can implement this question-gating system cleanly using Dialogic's built-in **Choice Conditions**.

### How it looks inside Dialogic's Timeline Editor:

```text
[Text] Brenda: "What do you want, suit?"

# Choice 1: Basic (Always Available)
[Choice] "What software are you running?"
    [Text] Brenda: "The enterprise portal. Obviously."

# Choice 2: Intermediate (Gated by SHADOW_AUDIT >= 2)
[Choice] [SHADOW AUDIT] "I see that custom spreadsheet open on screen 2..."
  [Condition] If {SHADOW_AUDIT} >= 2
    [Text] Brenda: "Fine... management doesn't know about this spreadsheet."
    # Reward player with progress!
    [Call Code] PlayerStats.add_xp("SHADOW_AUDIT", 1)

# Choice 3: Advanced (Gated by LEGACY_WHISPERER >= 4)
[Choice] [LEGACY WHISPERER] "How many batch requests can that AS/400 handle before locking?"
  [Condition] If {LEGACY_WHISPERER} >= 4
    [Text] Brenda: "Finally, someone who understands hardware limitations!"
    [Call Code] PlayerStats.add_xp("LEGACY_WHISPERER", 1)

```

---

## Recommended Next Planning Step

Before writing more Godot code, we can define the **Stat Matrix & Progression Rules**:

1. **Define 6–8 core starter stats** (e.g., *Legacy Whisperer, Shadow Audit, ROI Rhetoric, Human-in-the-Loop*).
2. **Establish stat unlock thresholds** (e.g., Level 0 = Basic questions, Level 2 = Intermediate questions, Level 4 = Advanced architectural questions).
3. **Map Day 1 locations/items** where players can interact with objects to gain their first stat points (e.g., inspecting a crash log grants +1 *Legacy Whisperer*).
