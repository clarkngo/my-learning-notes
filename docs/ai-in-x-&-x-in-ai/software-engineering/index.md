---
title: Software Engineering
layout: default
parent: AI in X & X in AI
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# AI-Infused SDLC

Here is a breakdown of the **AI-Infused SDLC** (GenAI SDLC).

In this model, the goal of the software remains the same (e.g., building a web app, a finance calculator, or a game), but the **tools** used to build it change from passive text editors to active AI collaborators.

### The Core Shift: From "Writer" to "Reviewer"
In a traditional SDLC, the developer’s brain is the bottleneck for syntax and logic. In an AI-Infused SDLC, the AI generates the "first draft" of almost everything, and the human moves up the stack to become the **Architect, Editor, and Auditor**.

---

### Phase 1: Requirements & Analysis
**The Goal:** Turn vague business ideas into technical specifications.

* **Traditional:** You spend hours in meetings, take notes, and manually write Jira tickets or PRDs (Product Requirement Documents). You might miss edge cases that aren't found until coding starts.
* **AI-Infused:**
    * **Synthesizing:** You feed raw meeting transcripts into an AI and ask it to "Extract functional requirements and acceptance criteria."
    * **Gap Analysis:** You ask the AI, "Act as a malicious user. Look at these requirements and tell me how you would exploit the system logic."
    * **Result:** Specs are generated 10x faster and pre-validated for logical consistency.

### Phase 2: Design & Architecture
**The Goal:** Plan how the software looks and how the systems talk to each other.

* **Traditional:** Designers draw wireframes in Figma. Architects manually draw diagrams in Visio/LucidChart.
* **AI-Infused:**
    * **Generative UI:** You describe a UI ("A dashboard for a finance app with a dark mode toggle and a data grid") and tools generate the actual React/HTML code or design mockups instantly.
    * **Architectural Critique:** You describe your tech stack to an AI ("I'm using Python Flask with SQLite for a high-traffic app") and it warns you about bottlenecks ("SQLite will lock during high concurrent writes; suggest migrating to PostgreSQL").

### Phase 3: Development (The "Copilot" Phase)
**The Goal:** Write the actual code.

* **Traditional:** You type character-by-character. You write "boilerplate" (setup code) manually. When you get stuck, you context-switch to Google or StackOverflow.
* **AI-Infused:**
    * **Predictive Coding:** IDE extensions (like GitHub Copilot) predict entire functions before you type them.
    * **Boilerplate Destruction:** You simply comment `// Create a REST API for user login`, and the AI writes the 50 lines of code required.
    * **Legacy Translation:** You paste a chunk of old "spaghetti code" and ask the AI to "Refactor this to be cleaner and explain what it does."

### Phase 4: Testing & QA
**The Goal:** Ensure the software works and doesn't break.

* **Traditional:** Developers hate writing Unit Tests, so they often skip them. QA teams write manual test scripts that break whenever the UI changes slightly.
* **AI-Infused:**
    * **Auto-Generated Tests:** The AI analyzes your code and writes the Unit Tests for you, often finding edge cases (e.g., "What happens if the input is negative?") that you missed.
    * **Self-Healing Tests:** If a button's ID changes from `#submit` to `#btn-submit`, AI-driven testing tools detect the change and update the test automatically instead of failing the build.

### Phase 5: Deployment & Documentation
**The Goal:** Ship the code and maintain knowledge.

* **Traditional:** Documentation is always outdated because no one wants to update it. Error logs are cryptic.
* **AI-Infused:**
    * **Auto-Documentation:** AI scans the codebase and generates the `README.md` and API documentation automatically.
    * **Root Cause Analysis:** When a deployment fails, the AI analyzes the logs and explains, "The build failed because the new library version conflicts with your dependency in line 42," rather than just showing a stack trace.

---

### Summary Comparison

| Phase | Traditional SDLC | AI-Infused SDLC (GenAI) |
| :--- | :--- | :--- |
| **Role of Human** | Creator / Typist | Reviewer / Orchestrator |
| **Speed** | Linear (limited by typing speed) | Exponential (limited by review speed) |
| **Knowledge Base** | Google / StackOverflow | Context-aware AI Assistant |
| **Testing** | Manual / Brittle Automation | Generative / Self-Healing |
| **Code Quality** | Consistent with dev's skill level | High potential, but risk of "bloat" |

### The "New" Risks
1.  **Hallucinations:** AI might import a library that doesn't exist or suggests an insecure coding pattern. If the human reviewer is lazy, this creates security holes.
2.  **Code Understanding:** Junior developers might generate code they don't understand, making it impossible for them to debug it later without AI help.
3.  **Looping:** Using AI to write code and AI to test that code can lead to false positives (the AI validating its own mistakes).

# "AI-Infused" Workflow Steps
Based on **"Generative AI in Software Development"** (Laster & Yahav) and the "AI-Enabled SDLC" industry standards that have emerged around it, here is the specific workflow.

The book moves away from the "Waterfall" or "Agile" loops and defines a new **"Context-First" Workflow**.

The core philosophy is: **Do not start coding until the AI understands the Context.**

### The 5-Step "AI-Infused" Workflow
*This is the tactical execution loop derived from the text’s chapters on Planning, Creation, and Maintenance.*

#### Step 1: Bootstrap the Context (The `spec.md`)
Instead of keeping requirements in Jira or your head, you must digitize them into a format the AI can read **inside the IDE**.
* **Action:** Create a `spec.md` (Specification) file in your project root.
* **The Prompt:** "I want to build a Python script that converts MIPS assembly to Hex. Create a detailed `spec.md` that outlines the functional requirements, edge cases (e.g., invalid opcodes), and input/output formats."
* **Result:** The AI generates a text file acting as the "Anchor" for all future code. You **review and edit** this file manually.

#### Step 2: The Architecture Plan (The `plan.md`)
Do not ask the AI to "write the code" yet. Ask it to "plan the work."
* **Action:** Create a `plan.md` file.
* **The Prompt:** "Read `spec.md`. Create a step-by-step implementation plan in `plan.md`. Break it down into small, logical tasks (e.g., 'Step 1: Define Opcode Dictionary', 'Step 2: Parse Line Logic')."
* **Result:** A checklist of small, verifiable tasks.
* **Why this matters:** This prevents the AI from hallucinating a massive, broken 500-line script. It forces it to "think" before acting.

#### Step 3: Iterative Implementation (The Build Loop)
Now you code, but you do it **one task at a time** from your plan.
* **Action:** Open a code file (e.g., `converter.py`).
* **The Prompt:** "Read `plan.md`. Implement **Task 1** only. Do not do Task 2 yet."
* **Human Role:** You review the code. If it works, you mark Task 1 as `[x]` in `plan.md`.
* **The "Context Reset":** If the chat gets too long or confused, you **delete the chat history**, open a new chat, and say: "Read `spec.md` and `plan.md`. We are now on Task 2."

#### Step 4: Verification & "Self-Healing"
Instead of writing tests *after*, you ask the AI to generate tests *for* the specific task it just finished.
* **Action:** Create a test file.
* **The Prompt:** "Generate a `pytest` unit test for the function you just wrote. Ensure it covers the edge case defined in `spec.md` where the user inputs a non-integer."
* **Self-Healing:** If the test fails, paste the error into the chat. "The test failed with this error. Analyze the code and fix it."

#### Step 5: The "Status Update" (Documentation)
Never leave the code without updating the map.
* **Action:** Update a `status.md` or the `README.md`.
* **The Prompt:** "Read the code changes we made. Summarize them into the `README.md` and explain how to run the new script."

---

### Summary of the "Laster" Workflow Shifts

| Traditional Step | AI-Infused Step | The "Laster" Rule |
| :--- | :--- | :--- |
| **Requirements** | **Bootstrap (`spec.md`)** | "If it's not in the context file, the AI doesn't know it exists." |
| **Architecture** | **Decomposition (`plan.md`)** | "Break complex logic into atomic tasks to prevent hallucinations." |
| **Coding** | **Iterative Context** | "Reset the AI context frequently to keep it smart." |
| **Testing** | **Property-Based Gen** | "Ask AI to find inputs that *break* the code, not just pass it." |

### Why this works for you (MIPS & Python)
Since you are interested in **MIPS Assembly** and **Python**, this workflow is powerful for translation projects:
1.  **Spec:** Paste a MIPS instruction manual snippet into `spec.md`.
2.  **Plan:** Ask AI to map every MIPS instruction to a Python function in `plan.md`.
3.  **Build:** Implement one instruction group (e.g., arithmetic) at a time.

# Textbooks and References
Since it is **November 2025**, you are in luck. The "AI-Infused SDLC" field has matured significantly in the last 18 months, moving from experimental blog posts to formal methodologies.

Here are the standard textbooks and references available right now that define this new way of working.

### 1. The "Big Picture" Textbooks (Methodology & Process)
These books cover the entire lifecycle, rewriting the rules of Agile/Waterfall for an AI world.

* **"Generative AI in Software Development: The AI-Enabled SDLC"** by Brent Laster & Eran Yahav (O'Reilly, released May 2025)
    * *Why this one:* This is arguably the definitive "textbook" for your question. It maps GenAI directly to every phase of the classic SDLC. It moves beyond just "coding" and covers planning, estimation, and the cultural shift of the "AI-Enabled" team.
* **"Generative AI for Software Developers"** by Saurabh Shrivastava et al. (Packt, released Oct 2025)
    * *Why this one:* A very recent release that focuses on the **architecture** of AI-infused apps. It bridges the gap between being a software engineer and an AI engineer, covering how to build *with* GenAI tools rather than just using them to write code.
* **"AI-Assisted Programming: Better Planning, Coding, Testing, and Deployment"** by Tom Taulli (O'Reilly)
    * *Why this one:* Good for a broader overview. It’s less "academic" and more practical, focusing on the workflow changes for individual developers and small teams.

### 2. The "Hands-On" Manuals (Coding & Architecture)
If you want to master the "Build" phase (Phase 3 in our previous discussion), these are the go-to references.

* **"Programming with GitHub Copilot"** by Brent Laster (O'Reilly)
    * *Why this one:* It’s not just a manual for the tool; it teaches **"Prompt Engineering for Logic."** It covers advanced patterns like using Copilot to refactor legacy monoliths and generating unit tests that actually compile.
* **"Refactoring with AI"** (Various authors/editions emerging in late 2024/2025)
    * *Concept:* Look for sections in these books on "Legacy Modernization." This is the killer use case for AI infusion—taking 10-year-old Java/MIPS code and having AI explain it, document it, and port it to Python/Go.

### 3. The "Ops & Quality" Guides (AIOps & Testing)
This is where the "Agentic" workflows live—using AI to fix the code AI wrote.

* **"Effective Software Testing with Generative AI"** (Manning or similar practical guides)
    * *Focus:* These resources teach you how to use LLMs to generate "Property-Based Tests" (finding edge cases humans forget) rather than just simple assertions.
* **"Platform Engineering with AI"**
    * *Focus:* Look for chapters on **Self-Healing Infrastructure**. This covers how to set up agents that read your Kubernetes logs and auto-suggest fixes.

### 4. Industry Standards (The "New Rules")
Textbooks move slower than the industry. For the absolute latest "Safe SDLC" rules, you must reference these live frameworks:

* **OWASP Top 10 for LLM Applications:**
    * *Reference for:* Security. If you are infusing AI, you are introducing new vulnerabilities (Prompt Injection, Insecure Output Handling). This is the "Bible" for securing that process.
* **Google's "Secure AI Framework" (SAIF):**
    * *Reference for:* Enterprise operations. It adapts standard security controls (like NIST) for AI-infused systems.

### Recommendation: Where to start?
If you only buy one, get **"Generative AI in Software Development" (O'Reilly)**. It is the closest match to your request for a "textbook" on the AI SDLC.
