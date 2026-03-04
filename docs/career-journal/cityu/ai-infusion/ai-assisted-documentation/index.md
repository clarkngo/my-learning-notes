---
title: AI Assisted Documentation
layout: default
parent: AI Infusion
---

It is better to provide a **specific example for each TT**.

While a generic template is "minimal" for you to create, it increases the "cognitive load" for students. If the prompt is too vague, students often provide poor inputs to the AI, get generic results, and view the task as busywork.

Providing a specific example in each lab ensures the AI gives a high-quality, architecturally sound summary that actually helps the student learn.

Here is the guide with the **specific values** you should swap out for each document:

---

### **Bonus: AI-Assisted Documentation**

**Objective:** Use Generative AI to help you articulate the architectural changes and key concepts you implemented in this lab.

**Instructions:**

1. **Select a Key Snippet:** Identify the most important code you modified in this lab.
2. **Prompt an AI:** Use a Large Language Model (e.g., Gemini or ChatGPT) with the following prompt:

> "I am a student completing a technical lab on **[Insert Lab Title]**. I have implemented **[Insert Feature]** using **[Insert Tool]**.
> Based on this code snippet: **[Paste your code here]**
> Please write a concise, professional 3-sentence summary for my GitHub README that explains the architectural role of this code and how it contributes to a 'Smart and Secure System'."

3. **Update your README:** Review the AI-generated explanation, edit for accuracy, and paste it into your repository's `README.md`.

---

### **Values to use for each Lab:**

To make this as easy as possible for you, use these specific pairings for the bracketed text in each document:

| Document | [Insert Lab Title] | [Insert Feature] | [Insert Tool] |
| --- | --- | --- | --- |
| **TT01** | Cloud Computing | a movie search frontend | React and Material-UI |
| **TT02** | Edge Computing | a mobile search interface | React Native and Expo Go |
| **TT03** | MERN Stack | a backend API for movie data | Node.js, Express, and MongoDB |
| **TT04** | DevSecOps | automated security scanning | GitHub Actions and OWASP ZAP |
| **TT05** | DevSecOps Deployment | a secure cloud deployment pipeline | Amazon EC2 and ngrok |
| **TT06** | Machine Learning | a movie recommendation engine | FastAPI and Scikit-learn |
| **TT07** | MLOps | experiment tracking and management | MLflow |
| **TT08** | LLM Chat | a local AI chat interface | Ollama and the Gemma model |
| **TT09** | RAG & Vector DB | a Retrieval-Augmented Generation flow | ChromaDB and LangChain |
| **TT10** | RAG & PDF | a PDF-based knowledge retrieval system | LangChain and PDF data loaders |