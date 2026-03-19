---
layout: default
title: Semantic Context Engineering
parent: AI Orchestration
---

# Lesson: Writing for Agents (AI-Ready Notes)

My note-taking has evolved. I no longer write notes just for "future me"; I write **Context Assets** designed for machine consumption.

### Best Practices
- **Plug-and-Play Feedback:** Feedback is gathered in a descriptive, structured format that can be directly pasted into a prompt.
- **Semantic Density:** I focus on reducing "noise" so the AI doesn't waste tokens on irrelevant data.
- **Zero-Loss Handover:** Notes serve as the bridge between my "Planning Mode" (manual) and "Build Mode" (AI).

**The Theory:** *Semantic Communication Theory.* Instead of transmitting "bits" (raw data), I transmit "meaning" (task-relevant information). This ensures a high **Signal-to-Noise Ratio**, making the AI's output significantly more accurate.

### **Theory: Semantic Signal-to-Noise Ratio (SNR)**
Standard "prompt engineering" is dead; **Context Engineering** is the 2026 standard. This theory focuses on maximizing the **Semantic Density** of the information you provide. By "cleaning" your feedback before plugging it into an AI, you prevent **Context Rot**—where the AI gets distracted by irrelevant details in its working memory.

### **Sample Scenario: The "Dirty Feedback" Clean-up**
You receive a chaotic email from a beta tester with 10 different complaints mixed with personal anecdotes.
* **The Action:** Instead of pasting the whole email, you use **Claude CLI** to "Semantically Filter" the text into a structured JSON or Markdown **Spec**. You then feed *that* refined context into your build agent.

### **The Process**
1.  **Raw Input Gathering:** Collect feedback, logs, or ideas in their messiest form.
2.  **Semantic Filtering:** Identify the core "Intent" vs. "Noise."
3.  **Spec-Structuring:** Format the intent into a machine-readable "Plug-and-Play" block (use `[SPEC START]` tags).
4.  **Injection:** Feed only the high-SNR context into the AI for the build.
