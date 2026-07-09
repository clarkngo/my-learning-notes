---
title: Research and Validate
layout: default
parent: IKW
---

Without upfront validation and structuring, non-coders fall into the **"Prompt Swamp"**—they ask the AI for a massive, complex app all at once, get back broken code, don't know how to debug it, and lose interest.

Teaching them to treat the AI like a **Product Manager first** and a **Junior Engineer second** keeps the momentum high and prevents frustration.

---

## 💡 Why Idea Validation Matters for Non-Coders

Non-coding students usually struggle with vibe coding for two reasons:

1. **Scope Creep:** Trying to build a full Boeing flight simulator in prompt #1.
2. **Ambiguity:** Asking for "a cool aviation app" and getting a generic, broken template.

By forcing a 10-minute **"Ideate & Validate"** phase before touching code, they learn **Specification-Driven Thinking**—breaking an idea down into core logic, wireframes, and user value before executing.

---

## ⏱️ Mini-Lecture Structure (Max 10–12 Minutes per Session)

To keep Hands-on-Screens (HOS) high, keep your mini-lectures tight, visual, and focused on **mental models**, not syntax.

```
┌─────────────────────────────────────────────────────────────┐
│ 10-12 min: Micro-Lecture & Live Demo (Hook + Technique)     │
├─────────────────────────────────────────────────────────────┤
│ 35-40 min: Hands-on-Screens (Build, Prompt, Iterate)        │
├─────────────────────────────────────────────────────────────┤
│ 10 min: Quick Demos / Peer "Vibe Checks"                     │
└─────────────────────────────────────────────────────────────┘

```

---

### Module 1 Mini-Lecture: *Idea Validation & The "Copilot" Mindset*

#### 1. Concept: The Product Spec (3 min)

* **Key Message:** "AI is an eager junior developer. If you give vague instructions, it builds garbage. Your job is to be the Product Manager."
* **Validation Framework:**
* **Problem:** What simple problem does this app/game solve?
* **User:** Who is playing/using it?
* **Core Loop:** What is the *one* primary interaction? (e.g., *Plane moves up/down to dodge weather hazards*).



#### 2. Live Demo: The "Good vs. Bad" Prompt (5 min)

* Show what happens with a weak prompt: *"Make me an aviation game."* (Show the bloated, buggy result).
* Show the **Structured Spec Prompt**:
> *"Act as a Lead Python Developer. I want to build a minimal 2D Pygame flight simulator. Core mechanics: 1 plane, arrow keys for altitude, random storm clouds moving left. Give me a working prototype in one file."*



#### 3. Challenge Intro (2 min)

* "Pick an aviation concept, validate the core loop with your AI assistant, and get your v0.1 prototype running."

---

### Module 2 Mini-Lecture: *Deconstruction & Multi-Disciplinary Features*

#### 1. Concept: Modular Expansion (3 min)

* **Key Message:** "Never ask AI to rewrite everything at once. Add one feature at a time, validate it works, then move to the next."
* **Cross-Disciplinary Roles:**
* *Aviation/Tech:* Validate the math/physics (e.g., fuel consumption rates).
* *Design:* Define UI color palettes, button placement, and visual feedback.
* *Business:* Add analytics, scoring, or cost models.



#### 2. Live Demo: The Feature Merge (5 min)

* Take the simple game from Day 1.
* Ask AI to brainstorm 3 features tailored to a non-technical major:
* `"Give me 3 simple business/fuel tracking ideas we can add to this Python script."`


* Prompt the AI to merge **just one** feature cleanly:
* `"Here is my working game script [paste]. Add a live Fuel Efficiency score in the top right corner based on speed. Do not alter the flight controls."`



#### 3. Challenge Intro (2 min)

* "Pair up across majors! Combine one technical feature with one business/design feature."

---

### Module 3 Mini-Lecture: *Debugging & The Demo Pitch*

#### 1. Concept: The "Error Loop" & Polish (3 min)

* **Key Message:** "Red text on screen isn't a failure—it's just fuel for the AI."
* **The Debugging Rule:** Copy error $\rightarrow$ Paste to AI $\rightarrow$ `"Fix this error and explain in 1 sentence what went wrong."`

#### 2. Live Demo: Turning Code into a Showcase (5 min)

* Show how to ask the AI to generate a **Pitch Script** based on the code built:
> `"Review our Python script [paste]. Generate a 60-second presentation script for non-coders highlighting our problem, our solution, and how we iterated the app."`



#### 3. Challenge Intro (2 min)

* "Final polish! Fix all bugs, finalize your UI, and prepare your 60-second live demo."

---

## 📋 Pre-Coding Idea Validation Cheat Sheet (Handout for Students)

Give students this 3-step checklist before they open their code editor:

| Step | Goal | AI Prompt Tool |
| --- | --- | --- |
| **1. Brainstorm** | Narrow down a broad idea to a micro-scope | *"Give me 3 ultra-simple Python game ideas based on [Aviation/Logistics] that can be built in under 100 lines of code."* |
| **2. Validate** | Ensure the logic makes sense before coding | *"I want to build [Idea]. What are the 3 core features it needs, and what should we intentionally leave out to keep it simple?"* |
| **3. Spec Out** | Generate the initial prompt structure | *"Write a detailed prompt I can feed back into you to generate the initial Python prototype for this idea."* |