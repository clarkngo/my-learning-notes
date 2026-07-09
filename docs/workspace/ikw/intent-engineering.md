---
title: Intent Engineering
layout: default
parent: IKW
---

To teach non-coding students how to "vibe code" effectively without letting them get lost in AI-generated chaos, you need to teach them how to think like a **product manager and system architect**, not a syntax writer.

Since they are not focusing on loops or variables, their core mechanics are about **intent, context boundaries, and managing the AI feedback loop**.

These are the **4 core components** to teach first, framed specifically for a non-technical, multi-disciplinary audience:

---

## 1. Intent Engineering (The "What" & "Why")

Non-coders often start by telling the AI *how* to build something rather than *what* problem to solve. They write prompts like, *"Make a button that calculates fuel."* The AI will build exactly that, but it won’t match the rest of the application.

* **What to teach:** Teach them to prompt with **Persona, Context, and Goal**.
* **The Blueprint:** A great vibe-coding prompt always follows a structure:
* *Role:* "Act as an expert data visualizer for airlines."
* *Goal:* "Build a slider that allows a pilot to change cargo weight and instantly see its impact on fuel consumption."
* *Constraint:* "Keep it simple and use a Streamlit layout."


* **The Lesson:** Clear intent prevents the AI from guessing and hallucinating random features.

## 2. Context Windows & "Slicing" (The Blueprinting)

When students get excited, they try to prompt the entire app at once: *"Build me an aviation simulator game with a login page, a database, a leaderboard, and realistic crosswinds."* The AI will choke, output incomplete code, or break.

* **What to teach:** The concept of **Vertical Slicing**. You build a software product one tiny, working feature at a time.
* **The Protocol:**
* *Slice 1:* Get a static box on the screen. (Test it. Does it work? Yes.)
* *Slice 2:* Make the box move with a slider. (Test it. Does it work? Yes.)
* *Slice 3:* Add data text below the box.


* **The Lesson:** Never ask the AI for a second feature until the first feature is fully working and tested.

## 3. The Guardrail State (Preservation & Context Management)

This is where non-coders get trapped. They have a working app, they ask the AI to add a new button, and the AI rewrites the entire code, completely breaking the first feature. The students don't know enough code to find what changed, so they get frustrated and give up.

* **What to teach:** **Context Preservation**. How to protect what already works.
* **The Protocol:** Teach them to instruct the AI explicitly: `"Here is my working code. Add a fuel tracker to the sidebar. Do NOT alter or change any of the existing physics logic in the main screen. Return the full, updated script."`
* **The Lesson:** You must actively defend your working code from the AI's tendency to over-rewrite.

## 4. The Loop-Back Protocol (Debugging via Error Trapping)

In traditional coding, a crash means staring at line 42 to find a missing comma. In vibe coding, a crash is just a data point to feed back into the loop.

* **What to teach:** Non-coders panic when they see red error text. You need to teach them to treat error messages as a conversational hand-off.
* **The Protocol:**
1. Do not read the code to fix it.
2. Copy the *entire* terminal error message.
3. Paste it back to the LLM with zero emotion: `"The script crashed with this error when I moved the slider. Fix the logic and provide the complete corrected code."`


* **The Lesson:** The AI is responsible for fixing its own syntax mistakes; the student is just the bridge passing the telemetry back to the pilot.

---

> ### 💡 The Mental Shift
> 
> 
> Summarize it for them like this on Day 1: **"You are no longer the bricklayer (the coder). You are the construction foreman. You don't lay the bricks; you check the blueprint, tell the worker what to build next, and tell them to rebuild it if it's crooked."**