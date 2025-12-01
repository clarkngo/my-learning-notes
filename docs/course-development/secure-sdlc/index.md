---
title: Secure SDLC
layout: default
parent: Course Development
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

Based on the syllabus provided, I have redesigned the prompts to ensure they strictly align with the **Course Outcomes (COs)** and the specific definition of **Hands-On Skills (HOS)** assignments.

According to the syllabus, **Hands-On Skills** are distinct from the larger Virtual Labs; they are focused exercises where students "learn specific programming languages, APIs, or tools" and account for 20% of the grade. The course also emphasizes balancing **usability and security**  and incorporating **encryption and access control**.

Here is the redesigned prompt strategy:

### **1. The Redesigned Master Prompt**
> "Act as a Senior AppSec Instructor for CS547. I need to generate 10 weekly 'Hands-On Skill' (HOS) exercises. According to the syllabus, these must be focused practice exercises involving specific programming languages (Python/Java), APIs, or security tools.
>
> **The exercises must map to these Course Outcomes:**
> 1.  **Policies & Patterns:** Identifying standard policies and design patterns.
> 2.  **Usability vs. Security:** Balancing threat mitigation with user experience.
> 3.  **Deployment & Updates:** Handling patches, releases, and real-time deployment issues.
> 4.  **Countermeasures:** Implementing specific defenses against vulnerabilities.
> 5.  **Full SDLC:** applying security from start to finish.
>
> For each week, provide a **Scenario**, a **Technical Task** (coding or tool use), and a **Deliverable**."

---

### **2. Weekly Prompt Suggestions (Aligned to Syllabus)**

#### **Module 1: Software Security & SDLC**
*Focus: Policies & Best Practices (Matches CO: 1)*
> "Create a HOS exercise where the student must write a simple 'Security Policy' as code. Ask them to use a Python script to enforce a password policy (length, complexity) against a list of sample strings. They must explicitly comment on which industry 'best practice' (e.g., NIST guidelines) their code is enforcing."

#### **Module 2: Secure Development Lifecycle**
*Focus: Tool Integration & Automation (Matches CO: 5)*
> "Design a task focusing on the 'Secure Software Life Cycle'. Ask the student to write a mock CI/CD configuration file (YAML). They must insert a specific 'linter' or static analysis tool step that blocks the build if syntax errors are found, demonstrating how to automate security gates early in the cycle."

#### **Module 3: Security Assessment**
*Focus: Usability vs. Security (Matches CO: 2)*
> "Create a coding challenge specifically addressing the syllabus requirement to 'balance between usability and security'. Ask the student to design a Python function for Two-Factor Authentication (2FA). They must implement a 'Remember this Device' logic, explaining in comments how this improves usability without completely sacrificing security."

#### **Module 4: Secure Architecture**
*Focus: Design Patterns & Access Control (Matches CO: 1)*
> "Develop an exercise on Secure Architecture. Provide a diagram of a web app and ask the student to implement a specific 'Design Pattern' in pseudo-code: The 'Principle of Least Privilege.' They must define a JSON role-based access control (RBAC) policy where a 'User' cannot access 'Admin' routes."

#### **Module 5: Secure Design and Development**
*Focus: Countermeasures & Input Validation (Matches CO: 4)*
> "Focus on 'evaluating appropriate countermeasures'. Provide a snippet of Java code vulnerable to Cross-Site Scripting (XSS). Ask the student to use an encoding API (like OWASP Java Encoder or Python `html.escape`) to sanitize the input. This fulfills the requirement to use specific 'APIs or tools'."

#### **Module 6: Security Test Plan and Execution**
*Focus: Testing & Debugging (Matches CO: 5)*
> "Design a 'Fuzz Testing' exercise. The syllabus mentions the need to 'test and debug programs'. Ask the student to write a simple Python script that generates 50 random malformed inputs and sends them to a specific function to see if it raises an unhandled exception (crashes)."

#### **Module 7: Secure Deployment**
*Focus: Patching & Updates (Matches CO: 3)*
> "Create a task centered on 'analyzing policies for implementing updates and patches'. Provide a list of software dependencies with version numbers. Ask the student to use a tool (like `pip audit` or `npm audit`) command concept to identify which version contains a specific CVE and write the command to update it safely."

#### **Module 8: Secure Post-Release Support**
*Focus: Real-time Deployment Issues (Matches CO: 3)*
> "Simulate a post-release scenario. The student must write a 'Hotfix' script. Provide a 'production' code snippet with a logic error. Ask the student to write the patch code and a rollback script (a script that restores the previous version) to demonstrate handling 'stability' issues mentioned in the syllabus."

#### **Module 9: Real-World Application**
*Focus: Encryption Features (Matches Course Description)*
> "The course description explicitly highlights 'encryption'. Create a HOS exercise where the student uses a cryptography library (like Python `cryptography` or `hashlib`) to properly hash user passwords with salt before 'storing' them. Ask them to compare the output of salted vs. unsalted hashes."

#### **Module 10: Strategy & Future Direction**
*Focus: Emerging Threats & APIs (Matches CO: 4 & HOS Definition)*
> "Create a final skill check on 'identifying common vulnerabilities' in modern environments (APIs). Ask the student to use a tool like Postman or `curl` (command line) to inspect the headers of a response. They must identify missing security headers (like HSTS or Content-Security-Policy) and write the code configuration to add them."

---

### **Why These Were Redesigned**
1.  **Emphasis on Usability:** Week 3 was adjusted because the syllabus explicitly lists "balance between usability and security" as a core outcome, which is often overlooked in pure coding drills.
2.  **Tool/API Usage:** The HOS definition in the syllabus specifically mentions "learning... APIs, or tools". The new prompts explicitly ask for standard libraries (APIs) and tools (Linters, curl, pip audit).
3.  **Deployment Focus:** Weeks 7 and 8 were tightened to focus on "updates, activations, patches", which is a specific outcome distinct from general coding.
4.  **Encryption:** Week 9 was adjusted to ensure "encryption," which is highlighted in the course description, is practiced hands-on.