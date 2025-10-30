---
title: Movie Multi Agent A2A MCP
layout: home
parent: Agent Engineering
---

Design a modern multi-agent system for a sophisticated movie application, showing exactly how A2A and MCP would function as the core plumbing.

This architecture is not just a single app; it's a team of specialized AI agents working together.

### The Agent Team for Your Movie App

First, here is the team of specialized agents that make up your **Multi-Agent System**:

1.  **Orchestrator Agent (The "Conductor"):**
    * This is the main agent the user interacts with.
    * It doesn't do the heavy lifting itself. Its job is to understand the user's request, break it down into sub-tasks, and delegate those tasks to the correct specialist agents.

2.  **Recommendation Agent (The "Cinephile"):**
    * **Specialty:** Understanding user taste and movie data.
    * **Task:** Finds and ranks movies based on user preferences, history, and mood.

3.  **Social Agent (The "Friend"):**
    * **Specialty:** Manages connections and social activity.
    * **Task:** Checks what friends are watching, manages watch parties, and facilitates group recommendations.

4.  **Booking Agent (The "Ticket-Bot"):**
    * **Specialty:** Interacting with theaters and payment systems.
    * **Task:** Finds showtimes, reserves seats, and purchases tickets.

---

### How They Use A2A and MCP: A User Scenario

Let's walk through a user request: **"Find a good sci-fi movie that my friend Clark and I can watch in a theater tonight after 7 PM."**

Here is the step-by-step flow, showing the A2A and MCP protocols in action.

#### **Step 1: The User's Request**

* **You:** "Find a good sci-fi movie that my friend Clark and I can watch in a theater tonight after 7 PM."
* **Orchestrator Agent:** Receives this complex request and breaks it into a plan:
    1.  I need to find sci-fi movies. (Task for **Recommendation Agent**)
    2.  I need to see what Clark likes. (Task for **Social Agent**)
    3.  I need to combine those lists to find a match. (My task)
    4.  I need to find showtimes for the matching movies. (Task for **Booking Agent**)
    5.  I need to present the final, filtered options to the user. (My task)

#### **Step 2: Finding Movie & Friend Preferences (Parallel Tasks)**

This is where the agent team works together.

* **Communication via A2A (Agent-to-Agent):**
    * **Orchestrator** → **Recommendation Agent**: "Task 1: Get top 10 sci-fi movie recommendations for User A."
    * **Orchestrator** → **Social Agent**: "Task 2: Get movie preferences for User B (Clark)."

* **Agents Use MCP (Agent-to-Tool) to Work:**
    * **Recommendation Agent** (working on Task 1) uses **MCP** to connect to its tools:
        * It calls the **IMDb/TMDB API** (via an MCP Server) to get a list of all sci-fi movies.
        * It calls the **User Profile Database** (via another MCP Server) to get your watch history.
        * It runs its internal logic and prepares a list of 10 movies you'd like.
    * **Social Agent** (working on Task 2) uses **MCP** to:
        * Call the **User Profile Database** to find "Clark" in your friends list.
        * Query Clark's public "Liked Movies" list (assuming he gave permission).

#### **Step 3: Combining Data (A2A)**

* **Communication via A2A:**
    * **Recommendation Agent** → **Orchestrator**: "Task 1 Complete. Here is the list: [Blade Runner, Dune, ...]"
    * **Social Agent** → **Orchestrator**: "Task 2 Complete. Clark likes [Dune, ...]"
* **Orchestrator Agent:** Compares the lists and finds a match: **"Dune"**.

#### **Step 4: Finding Showtimes (A2A & MCP)**

* **Communication via A2A:**
    * **Orchestrator** → **Booking Agent**: "Task 3: Find showtimes for 'Dune' tonight after 7 PM near User A."
* **Agent Uses MCP to Work:**
    * **Booking Agent** (working on Task 3) uses **MCP** to connect to its tools:
        * It calls the **Fandango API** (via an MCP Server) to get showtimes at local theaters.
        * It calls the **Google Maps API** (via an MCP Server) to filter by theaters near you.
    * **Booking Agent** finds two options: "AMC @ 7:30 PM" and "Regal @ 8:00 PM".

#### **Step 5: Final Response**

* **Communication via A2A:**
    * **Booking Agent** → **Orchestrator**: "Task 3 Complete. Here are the options: [AMC @ 7:30, Regal @ 8:00]"
* **Orchestrator Agent:** Assembles the final answer for you.
* **Orchestrator** → **You**: "Done. You and Clark both like 'Dune.' There are two showtimes nearby: AMC at 7:30 PM and Regal at 8:00 PM. Would you like me to book tickets?"

---

### Summary: Architecture Diagram

This table shows the clear separation of duties:

| **The Concept** | **What It Is** | **Example in This App** |
| :--- | :--- | :--- |
| **Multi-Agent System** | **The Architecture** (The Team) | The team of **Orchestrator**, **Recommendation**, **Social**, and **Booking** agents. |
| **A2A Protocol** | **Agent ↔ Agent** (The Team Communication) | The **Orchestrator** delegating tasks to the **Booking Agent** ("Find tickets for 'Dune'"). |
| **MCP Protocol** | **Agent ↔ Tool** (Doing the Job) | The **Booking Agent** connecting to the **Fandango API** to find those tickets. |