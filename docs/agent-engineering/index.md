---
title: Agent Engineering
layout: home
has_toc: false
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}


**Agent Engineering** is the overall field of building AI agents. The **Google ADK** is a toolkit to do that building. **A2A** and **MCP** are two separate, open-source *protocols* (like languages) that an agent built with the ADK uses to communicate.

---

## Agent Engineering (The "What")
This is the broad **software engineering discipline** focused on designing, building, and managing autonomous AI agents.

Think of it as the entire field of "AI agent architecture." It involves figuring out how an agent should think, what its goals are, what tools it can use, and how it should be supervised.

## Google ADK (The "Toolkit")
The **Agent Development Kit (ADK)** is an open-source framework, or toolkit, from Google that provides the scaffolding to actually *build* an AI agent.

If agent engineering is the discipline (like "civil engineering"), the ADK is the set of blueprints, tools, and pre-fabricated parts (like steel beams and concrete) that help you build your specific project (a "skyscraper agent"). It helps you handle the agent's logic, memory, and orchestration.

## Key Concepts: Architecture & Protocols
A **Multi-Agent System** (the architecture) *uses* **A2A** (for collaboration) and **MCP** (for action) to get work done.

### Multi-Agent System (The Architecture)

This is the overall *design* of your application. Instead of building one giant, all-knowing AI, you build a *team* of specialized, smaller agents that collaborate.

* **Analogy:** A company. You don't have one "CEO" who also does marketing, finance, and engineering. You have a **Multi-Agent System** made of a CEO, a CFO, and a CMO, who all work together.

### The Two Communication Protocols (The "Languages")
For an agent to be useful, it needs to talk to the outside world. A2A and MCP are two critical, complementary protocols that allow this. They are not competing; an agent needs both.

#### **A2A (The Agent-to-Agent Protocol)**

This is the *communication standard* that lets the agents in your system talk to **each other**. It's the "universal language" for agent collaboration.

* **Analogy:** The common language (e.g., English) that the CEO, CFO, and CMO use to talk *to each other* in a meeting. The CEO doesn't need to know *how* the CFO does the finances; they just need to use A2A to ask, "What's our current budget?"

#### **MCP (The Model Context Protocol)**

This is the *communication standard* that lets a single agent talk to its **tools and data**. It's the "universal remote" for interacting with the outside world.

* **Analogy:** The standardized software (e.g., Excel, Salesforce) that the specialists use to do their actual job. When the CFO gets the A2A request for the budget, they use **MCP** to securely connect to the company's financial database (a *tool*) to get the answer.

---

### Putting It All Together: A Scenario

Let's say you're building a **Multi-Agent System** (the architecture) to plan a company offsite.

1.  Your **"Planner Agent"** (the CEO) kicks off the task.
2.  The Planner Agent uses **A2A** (the *agent-to-agent language*) to ask the **"Calendar Agent"** (a specialist), "Find a 3-day window in November when the engineering team is free."
3.  The **Calendar Agent** receives the A2A request. It then uses **MCP** (the *agent-to-tool language*) to connect to the Google Calendar API (a *tool*) to get the data.
4.  The Calendar Agent sends a reply via **A2A** back to the Planner Agent: "November 5-7 is free."
5.  The Planner Agent now uses **A2A** to ask the **"Travel Agent"**: "Book flights to Seattle for 10 people for Nov 5-7."
6.  The **Travel Agent** receives the A2A request. It then uses **MCP** to connect to the United Airlines API (a *tool*) and the Amex API (another *tool*) to complete the booking.
7.  The Travel Agent reports back to the Planner via **A2A**: "Done."

Here is a simple table to help you remember:

| Concept | **Multi-Agent System** | **A2A Protocol** | **MCP Protocol** |
| :--- | :--- | :--- | :--- |
| **What is it?** | The **architecture** or design pattern. | A **communication protocol** (a standard). | A **communication protocol** (a standard). |
| **Who talks?** | N/A (It's the *system* itself) | **Agent** $\leftrightarrow$ **Agent** | **Agent** $\leftrightarrow$ **Tool / API / Database** |
| **Analogy** | A **team of specialists** (CEO, CFO, CMO). | The **shared language** they use *with each other*. | The **standardized software** (Excel) they use *to do their work*. |
| **Key Goal** | Collaboration & Specialization | **Interoperability** (letting agents from different companies/frameworks work together) | **Tool Use** (letting an agent securely use any tool or data source) |

[This short video offers a good explanation](https://www.youtube.com/watch?v=q-TWU55reI8) of how MCP works as the "secret weapon" for AI engineers.