---
title: Sessions Mentee Justine
layout: default
parent: Mentee Justine
---


# Sessions

## 12-15-2025
**Meeting Notes: Mentorship Session #4 with Justine**

**Session Overview**
The session began with a review of outstanding tasks, noting that the previously assigned process flow documentation was not completed. The discussion shifted to addressing **technical gaps in PowerBI and Power Query**, specifically regarding Justine's reliance on AI trial-and-error for building complex formulas. Clark and Justine explored strategies to improve efficiency using "Meta Prompting" and established a "small wins" approach to documentation.

**Key Discussions**
*   **Power Query & Technical Gaps:** Justine described their workflow in Power Query as similar to Excel formulas, used primarily for pulling data, creating calculated columns, and filtering measures. However, they struggle with complex queries—specifically **date ranges and time buckets** (e.g., Year-to-Date vs. Full Year comparisons)—often relying on iterative prompting with ChatGPT to find solutions.
*   **AI Prompt Engineering Strategies:** Clark advised moving away from inefficient, repetitive prompting. Instead, once a solution is found via trial-and-error, Justine should ask the AI to generate a **"Master Prompt," "Meta Prompt," or "One-shot Prompt"**. This template can be used to solve similar problems immediately in the future without needing multiple iterations.
*   **Documentation Philosophy:** To overcome procrastination on documentation (like the refresh error fix or process flows), Clark emphasized starting with **"small wins."** For example, listing steps in text before making a diagram, or creating a diagram with only three components to build momentum.

***

**Action Items**

*   **Document the Refresh Error:** Create a text-based list of the steps Andrew used to fix the report refresh error, then share it with the team.
*   **Create "Common Gotchas" Log:** Start documenting recurring technical struggles, specifically focusing on **date ranges and time period comparisons** in Power Query, to track and close knowledge gaps.
*   **Implement Meta Prompting:** After solving a complex problem with AI, explicitly ask the AI to provide a **"prompt template" or "one-shot prompt"** that creates the correct context for similar future issues.
*   **Run AI Retrospectives:** When a prompt fails initially, ask the AI for a "gap analysis" or "retrospective" to understand the difference between the initial request and the final working solution.
*   **Start the Process Flow:** Begin the process flow assignment by mapping out just **three components and two arrows** rather than attempting the entire architecture at once.

***

To clarify the AI strategy discussed, think of your current workflow like **guessing a lock combination**—you keep trying numbers until it opens (trial-and-error). The "Meta Prompt" strategy Clark suggested is like asking the lock maker for the **master key** once you've opened it; next time you encounter that specific type of lock (problem), you don't have to guess the numbers again, you just use the key.

## 12-8-2025
The mentorship session focused on refining the understanding of API fundamentals, M code, and data flow analysis within Power BI, specifically using Big Commerce environments. To track areas for deeper focus, the mentor advised the user to create issues in a repository.

### API Connection and Environment Details

For the particular Power Query implementation discussed, the connection to the Big Commerce APIs specifically utilized only the **access token** and the **store hash**, bypassing the need for a client ID or client secret.

Key details about the connection parameters include:

*   **Store Hash:** This serves as the unique identifier for a Big Commerce web store front end. It is consistent and does not change even if new API tokens are generated.
*   **Access Token:** This is a unique token generated when an API is created on Big Commerce.
*   **Data Sources:** The project involves connecting to two distinct environments, "Big Commerce EC" (Emergency Care) and "Big Commerce Sage," which function as separate systems.

### Data Flow, M Code, and Merging

The M code was used to connect to these two different API sources and process their respective data. A key step in the process was performing a **merge** operation on the M code side to combine the data from both systems. The merged output results in a single table for the end user in Power BI, with an added field or column that labels whether the data originated from EC or Sage.

### Account Hierarchy and Security Gaps

A notable point of confusion revolved around the API token generation versus its usage:

1.  The user generated the API token using their personal Big Commerce account, as they were the authorized individual.
2.  However, the Power BI data flow that refreshes the data uses the company's **service account**.
3.  Because the service account executes the data flow, team members with access to that account can view the connection parameters (store hash and access token).

The mentor raised important security research points for the user, asking them to investigate:

*   Whether the API token will still function if the user account that originally generated it is deleted.
*   Whether the generated token expires, and if setting an expiration policy would be considered a security best practice.

### System Diagramming and Architecture

The mentor advised using visual tools to document the system architecture:

*   **Process Flow Diagram:** Recommended for showing how the data comes from various sources and subsequently merges.
*   **Sequence Diagram:** Introduced as a detailed diagramming tool to illustrate the back-and-forth communication flow between different systems during an API call (e.g., how the front end requests data from the back end, which then requests data from the database).

The mentor also stressed the importance of verifying assumptions, such as whether the M code automatically utilizes a `GET` request, by consulting documentation or AI tools to ensure stakeholders receive answers based on certainty rather than speculation.

## 12-1-2025

**Meeting Notes: Mentorship Session #2**

| Detail | Information |
| :--- | :--- |
| **Date:** | December 1, 2025 |
| **Attendees:** | Clark Ngo, Justine |
| **Focus:** | Addressing technical gaps, framework development, and pair programming on action items |

***

### 1. Review and Session Structure

*   Justine noted that she was unable to complete prior action items, which was acceptable as the session was planned to accommodate this.
*   The meeting quickly switched to a "pair program" or working session structure to address action items together.
*   The standing action items were to develop a plan for addressing **technical gaps** and a **framework for discovering topics**.

***

### 2. Discussion on Learning Style and Technical Gaps

*   **Learning Preference:** Justine prefers a **hands-on, interactive** learning style. The best way for her to learn is to "follow along" with the examples provided in the materials, whether it's a book or video.
*   **Target Technologies:** The existing technologies mentioned were **PowerBI** and **SQL**. Justine also expressed interest in learning how to create a template framework for utilizing an **external API**, which might involve using **Python** if the PowerBI route fails.
*   **Current Success/Focus Area:** A previous collaboration with a coworker resulted in a working solution where the coworker created a **data flow** from an API using **PowerBI online** (not the desktop version). This solution uses dynamic dates and is currently grabbing 24 data points.
    *   The creation of this data flow was described as "almost very low code," similar to a drag-and-drop process like Power Automate.
*   **Justine's Specific Gap:** Justine can read and edit existing frameworks once they are laid out, but she lacks the knowledge to recreate or start the code/framework from scratch.

***

### 3. Strategy for Addressing Technical Gaps (API/Data Flow Focus)

Clark outlined a structured plan focusing on understanding the existing coworker's API-to-table data flow solution:

1.  **Understand and Document:** Document the existing process in GitHub (making sure to exclude company-specific data). This helps in building a mind map of how to create a template.
2.  **Go Deeper:** Understand and document each field or attribute.
3.  **API Fundamentals:** Understand the scope of **API technology**, what it allows, and what it doesn't allow. Talk to both technical and non-technical coworkers to gain understanding from two different paradigms.
4.  **Analyze Data Flow API:** Apply the same analytical approach to the data flow API solution.
5.  **Quantify Process Steps:** Identify the sequential steps in the process (e.g., use an element, request the URL resource, then parse). Listing these steps helps:
    *   Communicate the solution intuitively to coworkers or stakeholders.
    *   Identify steps that could be removed, improved, or justify an alternative solution (like using Python).
6.  **Documentation Structure:** Create two sections for documentation: a "quick setup" section for experienced users who only need a quick reminder, and a section for deep understanding (for new colleagues or when details are forgotten). Justine agreed that recreating the documentation is the best way to start, as she anticipates calling APIs again in the future.

***

### 4. Working Session (Drafting Documentation)

*   Justine shared her screen and began drafting documentation notes in GitHub/docs.
*   She used the structure: `## PowerBI` and `### high level`.
*   **Initial Notes Drafted:**
    *   The solution used **M code**, also known as the Advanced Query Editor.
    *   The M code allowed editing anything plug-and-play without going through each step individually.
    *   **Scenario One:** The process involved two APIs with two different API tokens (from two different sites), requiring the use of a variable separated by names (e.g., API token for this one, API token for another one).

***

### 5. New Action Item

*   Clark created a task header at the bottom of Justine's draft documentation.
*   **Task (Checkbox format):** Understand the system architecture of the data flow solution.
    *   *Note:* Justine should find something interesting in that space and keep going.


##  11-24-2025

The meeting summarized in the provided excerpts was the **first mentoring session** between the mentor and the mentee, Justine.

**Mentee's Goals and Challenges:**

*   **Need for Direction:** Justine sought mentorship because she needs more direction and struggles with personal development.
*   **Scope of Development:** She identified her desired development as professional, covering both career advancement and internal growth within her company.
*   **Uncertainty Due to Manager Change:** Due to a recent manager change, Justine is currently uncertain about her career goals or objectives starting next year.
*   **Professional Aspirations:** For the next two years, Justine intends to remain an **individual contributor** (IC) and is not yet looking to become a manager, as she feels she lacks the necessary skill set. She aims for a higher IC role, such as a Lead Data Analyst, potentially leading to higher pay.
*   **Technical Gaps:** She wants to be more proficient in her current role, which involves Power SQL, and feels she is "lacking" in areas like bug fixes.
*   **Year-End Goal Driver:** The primary driving factor for seeking a mentor is that it is one of her **year-end goals** at the company to intentionally find one. She noted that failing to complete this goal would feel incomplete, especially regarding performance review competitiveness.

**Session Focus and Short-Term Objectives:**

*   **Two-Week Goal:** Justine identified a small win for the next two weeks: successfully pulling a data flow into a current report, as she is building a new system.
*   **Addressing Gaps:** The discussion quickly focused on Justine's self-admitted **technical gaps**, particularly concerning a collaboration project involving capturing data flow refresh times using tools like Power Automate.
*   **Need for a Plan:** The mentor emphasized the importance of developing a clear plan for addressing these technical gaps, whether through textbooks, courses, or leveraging AI. Justine struggled with this, noting that she has not yet narrowed down the topics she needs to work on, making it difficult to utilize available resources effectively.

**Activities and Assignments:**

The mentor guided Justine through an activity to create output and establish a foundation for defining goals, utilizing AI tools and GitHub:

1.  **Portfolio Setup:** They successfully created and deployed an initial personal portfolio website on GitHub Pages using an AI tool (ChatGPT/Code Assist) to generate the initial `index.html` file.
2.  **Profile Creation:** They attempted to create a linked `menty-profile.html` file to build a personalized framework for exploring professional and personal growth topics, though technical troubleshooting was required during the session.
3.  **Documentation Site:** They set up a separate "personal notes" repository designed to be an easy-to-update documentation site (using markdown) that can serve as a reference toolbox for professional or personal learning. This site was successfully deployed and acts as a place to capture "small goals" and knowledge, such as common PowerBI steps or "gotchas".

The session concluded with **two main assignments** for the mentee before the next meeting:

1.  Develop a **plan on addressing technical gaps** (researching AI, references, or textbooks).
2.  Find a **framework for discovering topics** or identifying gaps.

The mentee was also instructed to update the newly created notes or portfolio website with anything learned.

***

*Analogy:* The initial mentoring session was like laying the foundation and framing the walls of a new house; Justine knows she needs a house (a mentor/direction) and has a general idea of its size (IC role), but the detailed blueprints (specific goals and skill gaps) still need to be drawn up, so the mentor provided the first tools (the GitHub portfolio and notes site) to start gathering the necessary materials and defining the architecture.