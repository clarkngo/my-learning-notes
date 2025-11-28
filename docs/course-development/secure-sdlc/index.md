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



### **Master Prompt (To generate the whole curriculum outline)**
> "Act as a Senior Application Security Engineer and Instructor. Based on the attached syllabus for CS547, design a 10-week 'Hands-On Skills' curriculum. For each week, propose a specific, interactive coding or analysis lab that takes 30-60 minutes to complete. The labs must align with the specific SDLC phase listed in the schedule (e.g., Week 4 is Architecture, Week 7 is Deployment). Focus on Python and Java examples. Output the title, learning objective, and a brief summary of the activity for each week."

---

### **Weekly Breakdown (To generate specific content per week)**

#### **Module 1: Software Security & SDLC**
*Focus: Introduction & The "Why"*
> "Create a hands-on exercise for Week 1 titled 'The Cost of Insecure Code.' Provide two Python scripts: one that is vulnerable to integer overflow or buffer overflow concepts, and one that is secure. Ask students to run both with specific inputs to crash the vulnerable one. Then, have them write a short reflection on how this bug could impact a business if found in production."

#### **Module 2: Secure Development Lifecycle (SDLC)**
*Focus: Process & Gates*
> "Generate an interactive lab for Week 2. Provide a diagram or text description of a standard 'Waterfall' SDLC. Ask students to identify where security 'gates' are missing. Then, provide a code snippet simulating a CI/CD pipeline configuration (YAML) and ask them to insert a 'SAST' (Static Application Security Testing) step in the correct location."

#### **Module 3: Security Assessment**
*Focus: Abuse Cases & Requirements*
> "Create a 'Hacker vs. Developer' exercise. Provide a list of functional requirements for a banking application login screen. Ask the student to write three specific 'Abuse Cases' (e.g., 'Attacker attempts to brute force password'). Then, have the student write the specific secure coding requirement (e.g., 'System must lock out account after 3 failed attempts') to mitigate that abuse case."

#### **Module 4: Secure Architecture**
*Focus: Threat Modeling (referencing Shostack reading)*
> "Design a Threat Modeling lab using the STRIDE methodology. Provide a Data Flow Diagram (DFD) for a simple e-commerce checkout system. Ask the student to identify one threat for 'Spoofing' and one for 'Tampering' in the diagram. Provide a checklist of mitigations and ask them to select the correct architectural fix for the identified threats."

#### **Module 5: Secure Design and Development**
*Focus: Input Validation & Injection*
> "Develop a 'Fix the Code' challenge for Week 5. Provide a Java code snippet containing a SQL Injection vulnerability (using string concatenation). Ask the student to identify the specific line causing the vulnerability and re-write the code using 'PreparedStatement' or parameterized queries to fix it."

#### **Module 6: Security Test Plan and Execution**
*Focus: Code Review (referencing Howard reading)*
> "Create a 'Security Code Review' simulation. Provide a 50-line Python web route that handles user file uploads. Deliberately include three vulnerabilities: missing file type validation, lack of size limits, and saving files with original user-provided names. Provide a checklist (based on Michael Howard's principles) and ask the student to mark the code where these violations occur."

#### **Module 7: Secure Deployment**
*Focus: Environment Hardening & Patching*
> "Create a hands-on lab regarding dependency vulnerabilities (referencing the Nappa et al. reading). Provide a sample `package.json` (Node.js) or `requirements.txt` (Python) file containing outdated libraries with known CVEs. Ask the student to use a tool concept (like OWASP Dependency Check) to identify which library is vulnerable and suggest the version number to upgrade to."

#### **Module 8: Secure Post-Release Support**
*Focus: Incident Response*
> "Simulate a 'Zero-Day' event. Provide a mock security alert stating: 'Critical Vulnerability found in Log4j.' Give the student a list of 5 server logs. Ask them to grep/search the logs to find evidence of an attempted exploit string. Then, ask them to draft a 3-sentence 'Patch Notification' email to customers."

#### **Module 9: Real-World Application**
*Focus: Defense in Depth*
> "Create a multi-layered security challenge. Provide a snippet of code for an API endpoint. Ask the student to apply three layers of security: 1. Rate Limiting (pseudo-code), 2. Input Validation (regex), and 3. Output Encoding (to prevent XSS). Explain how these layers work together."

#### **Module 10: Strategy & Future Direction**
*Focus: DevSecOps Automation*
> "Design a capstone lab involving a 'DevSecOps Pipeline.' Provide a broken GitHub Actions workflow file. The workflow pushes code to production even if tests fail. Ask the student to modify the script to block the build if the security scanner reports 'High' severity issues."