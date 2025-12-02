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

# Version 2

To recreate this exact style of interactive lab for future weeks (or different topics), use the following **Master Prompt**.

You can copy and paste this into a new chat with me (or another AI) to generate the next exercise instantly.

***

### **The Master Prompt**

> **Act as a Senior Application Security Engineer and Instructor.**
>
> I need you to generate a **standalone HTML file** for an interactive secure coding exercise. This file must use **PyScript** to run Python code directly in the browser (client-side only, no backend).
>
> **1. The Goal:**
> Create a single-file HTML lab for **[INSERT TOPIC HERE, e.g., Week 2: CI/CD Security]**.
>
> **2. Technical Requirements (Strict Adherence):**
> * **Single File:** All CSS, JavaScript, and Python logic must be embedded in one `.html` file.
> * **Library:** Use PyScript (release `2023.05.1`).
> * **Layout:** Use a **Split-Screen** layout (Instructions on the Left, Code Editor on the Right).
> * **Editor Features:**
>     * Custom `<textarea>` with line numbers that sync on scroll.
>     * Tab support (inserts 4 spaces).
>     * Shift+Tab support (un-indents).
>     * Autosave to `localStorage` (debounce 3 seconds).
> * **Console:** A black div (`#console-output`) for output. **Redirect all Python `print()` statements** to this div using a custom `sys.stdout` class. Do not show browser alerts.
> * **PDF Export:** Include a button to export a report using `jspdf`. The PDF must include the Student Name, Date, Status (Passed/Failed), and the code they wrote.
> * **Loading Screen:** Include the "Hacker Boot Sequence" animation (green text on black) that hides automatically when PyScript is ready.
>
> **3. The Content Structure:**
> * **Left Panel:**
>     * Title: "Exercise: [Title]"
>     * Scenario: A realistic business context for the vulnerability.
>     * Task: Step-by-step instructions.
>     * "Why This Matters": A brief educational takeaway.
> * **Right Panel (Python Editor):**
>     * Provide **broken/vulnerable code** by default.
>     * The user must modify a specific function to fix it.
> * **Grading Logic (`<py-script>`):**
>     * Write a `run_tests()` function.
>     * It must execute the user's code using `exec()`.
>     * It must run at least **two test cases**:
>         1.  **Normal Case:** Proves the code still works (e.g., valid input).
>         2.  **Attack Case:** Simulates the specific vulnerability (e.g., negative input, SQL injection string).
>     * If the Attack Case returns a safe result (or raises a caught error), update `window.submissionStatus` to "PASSED".
>     * If the Attack Case succeeds (exploits the bug), log "VULNERABILITY DETECTED" and set status to "FAILED".
>
> **4. Specific Exercise Details for THIS Request:**
> * **Vulnerability:** [Describe the specific bug, e.g., Hardcoded Credentials]
> * **Test Case Logic:** [Describe how to verify the fix, e.g., check if the variable 'password' is still in the source code]
>
> **Generate the complete HTML code now.**

***

### **How to Use This**
When you are ready for **Week 2**, you just paste the prompt above and fill in section **#4** like this:

> **4. Specific Exercise Details for THIS Request:**
> * **Vulnerability:** Insecure CI/CD Pipeline Configuration (Simulated with YAML or Python dict).
> * **Test Case Logic:** Check if the user added a "SAST Scan" step to the build dictionary before the "Deploy" step.

# Version 1

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

### Hands-on Skills Weekly Breakdown
Since you are building **interactive HTML webpages** to host these skills yourself, the technical constraints of the browser are just as important as the pedagogical goals. You need languages that are easy to run or simulate directly in the browser without a complex backend server.

### **Proposal: The "Browser-Native" Polyglot Approach**

I recommend using **Python** as the primary language for logic/process skills (70%), and **SQL/JavaScript** specifically for the web-attack skills (30%).

**Why this combination?**
1.  **Python** is concise and readable. You can easily embed a real Python environment in your HTML pages using libraries like **PyScript** or **Pyodide**, allowing students to run code directly in the browser.
2.  **SQL & JavaScript** are mandatory for Module 5 (Injection) because you cannot teach "SQL Injection" or "Cross-Site Scripting (XSS)" without them.
3.  **Avoid Compilers:** Avoiding C++ or Java for the *interactive* parts saves you from needing a heavy backend compiler.

---

### **Weekly Language Breakdown**

Here is the proposed language for each module’s Hands-On Skill (HOS), designed to work within your HTML build:

#### **Week 1: Software Security (Introduction)**
* **Language:** **Python**
* **Why:** You can provide a simple script with an "integer overflow" logic error.
* **HTML Interaction:** A text editor box where students change `x = x + 1` to `if x < MAX: x = x + 1`.
* **Validation:** Your page checks if the code output stays within limits.

#### **Week 2: Secure Development Lifecycle (SDLC)**
* **Language:** **YAML** (Simulated)
* **Why:** SDLC automation usually happens in configuration files (GitHub Actions / GitLab CI).
* **HTML Interaction:** A text area showing a broken pipeline.
* **Validation:** Use JavaScript Regex to verify the student added the string `- run: security-scan` in the correct location.

#### **Week 3: Security Assessment**
* **Language:** **Python**
* **Why:** Focus on "Logic" (Authentication). Python reads like pseudocode, making it easy for students to focus on the *logic* of a "Lockout Policy" rather than syntax.
* **HTML Interaction:** Students write a `check_password()` function.
* **Validation:** Run their function in-browser with Pyodide against 5 test cases.

#### **Week 4: Secure Architecture**
* **Language:** **JSON** (Policy Definition)
* **Why:** Architecture is about rules, not algorithms.
* **HTML Interaction:** Display a "User Role" JSON object. Ask students to delete `"admin": true` or change permissions.
* **Validation:** Simple JSON parsing in JavaScript to check if the dangerous permission was removed.

#### **Week 5: Secure Design (The "Injection" Week)**
* **Language:** **SQL** & **HTML/JavaScript**
* [cite_start]**Why:** You **must** use these to teach Injection[cite: 250].
* **HTML Interaction (Task A):** A mock SQL query box. Student types `' OR '1'='1` to bypass a login.
* **HTML Interaction (Task B):** An input box vulnerable to XSS. Student types `<script>alert(1)</script>`.
* **Validation:** JavaScript on your page checks if the input string contains the specific attack vector.

#### **Week 6: Testing & Execution**
* **Language:** **Python**
* [cite_start]**Why:** Python is the industry standard for writing "Fuzzers" and test scripts[cite: 252].
* **HTML Interaction:** Students write a loop that generates random characters to crash a mock function.
* **Validation:** Verify the loop runs at least 100 times and generates specific "bad" inputs.

#### **Week 7: Secure Deployment**
* **Language:** **Bash / Shell** (Simulated)
* **Why:** Deployment security is about command-line tools and patching.
* **HTML Interaction:** A fake "Terminal" window (black background, green text).
* **Validation:** Student types `npm audit fix` or `pip install --upgrade`. You simply match the string they typed.

#### **Week 8: Post-Release Support**
* **Language:** **Python**
* **Why:** Incident response often involves parsing logs.
* **HTML Interaction:** specific Regex construction.
* **Validation:** Student types a Regex pattern to find "credit card numbers" in a text block.

#### **Week 9: Real-World Application (Cryptography)**
* **Language:** **Python**
* **Why:** Python has standard libraries (`hashlib`) that are easy to demonstrate "Hashing" vs. "Encryption".
* **HTML Interaction:** Student uses `hashlib.sha256(password + salt)` to fix a vulnerability.
* **Validation:** Check if their output matches the expected secure hash.

#### **Week 10: Future Direction (APIs)**
* **Language:** **HTTP / cURL** (Simulated)
* **Why:** Modern security is about API headers.
* **HTML Interaction:** A tool that looks like "Postman" or a Terminal.
* **Validation:** Student must add a header `Content-Security-Policy: default-src 'self'`.

---

### **Technical Tip for your HTML Build**

Since you are building these yourself:
1.  **For Python Weeks:** Use [PyScript](https://pyscript.net/). It allows you to place a `<py-script>` tag in your HTML, and the code runs entirely in the user's browser. You don't need a server.
2.  **For Validation:** You don't always need to "run" the code. For many security exercises, **Regular Expressions (Regex)** in JavaScript are enough to check if the student found the right answer (e.g., did they type the correct SQL injection string?).

### **Why These Were Redesigned**
1.  **Emphasis on Usability:** Week 3 was adjusted because the syllabus explicitly lists "balance between usability and security" as a core outcome, which is often overlooked in pure coding drills.
2.  **Tool/API Usage:** The HOS definition in the syllabus specifically mentions "learning... APIs, or tools". The new prompts explicitly ask for standard libraries (APIs) and tools (Linters, curl, pip audit).
3.  **Deployment Focus:** Weeks 7 and 8 were tightened to focus on "updates, activations, patches", which is a specific outcome distinct from general coding.
4.  **Encryption:** Week 9 was adjusted to ensure "encryption," which is highlighted in the course description, is practiced hands-on.