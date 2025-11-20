---
title: Enhancing Professional Discourse
layout: default
parent: Research
---
# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

### Research Project: Enhancing Professional Discourse

**Title:** Enhancing Professional Discourse: A Multi-Agent Simulation for Real-Time Feedback on Collaborative Contribution

**Abstract**
Effective workplace communication requires more than linguistic fluency; it demands the situational awareness to know when and how to interject in ongoing multi-party dialogues. This research introduces a "Safe-to-Fail" simulated environment where a learner interacts with two autonomous AI agents acting as co-workers. The system utilizes a Real-Time Value Assessment Engine to evaluate the learner's contributions based on relevance, originality, and constructive impact. Furthermore, the system employs a Generative Scaffolding Module to provide "just-in-time" assistance, offering improved phrasing or follow-up inquiries. This study aims to measure the effectiveness of simulated conversational rehearsal in improving the confidence and "signal-to-noise" ratio of junior professionals in collaborative settings.

## Technical Debugging Scenario
### Conceptual Framework: How the "Value Assessment" Works

Since the core of your idea is determining if the user is "adding value," we need to define what "value" means for the AI to grade it. Here is a proposed rubric for the AI:

#### 1. The Simulation Loop
* **Agent A (AI):** Initiates a problem statement (e.g., "The server latency is increasing.")
* **Agent B (AI):** Responds with a theory (e.g., "It might be the database locks.")
* **User (Human):** *[Opportunity to input text]*

#### 2. The Value Matrix (The Scoring Logic)
When the user types their message, the AI analyzes it against these three criteria:

| Criteria | Low Value (0 Points) | High Value (1 Points) |
| :--- | :--- | :--- |
| **Relevance** | "I like pizza." (Off-topic) or "That's bad." (Passive) | "Have we checked the logs?" (Directly addresses the topic) |
| **Additivity** | "I agree with Agent B." (Redundant/Echo chamber) | "If it's DB locks, we should check the slow query log." (Adds new info) |
| **Actionability** | "We should fix this." (Vague) | "I can run a diagnostic script right now." (Proposes a next step) |

#### 3. The Assistance Module (The "Coach")
If the user scores **Low Value**, the AI Assistant intervenes *privately* (side-bar context) before the chat continues:
* *User Input:* "I agree."
* *AI Coach Feedback:* "Simple agreement doesn't move the meeting forward. Try asking **why** they think it's the database, or propose a way to verify it."
* *Suggested Prompt:* "What specific metrics are pointing you toward database locks?"

Here is a detailed **Technical Debugging Scenario** designed for your research framework.

### Scenario: The "500 Error" Spike
**Context:** You are a developer on a team maintaining an E-commerce API. It is 2:00 PM on a Friday. Suddenly, the monitoring dashboard lights up red.

**The Characters:**
* **Alex (AI Agent 1):** Senior Backend Lead (Focused on code/logic).
* **Sam (AI Agent 2):** DevOps Engineer (Focused on infrastructure/logs).
* **You (User):** Full Stack Developer.

---

### Phase 1: The Conversation Starts
The simulation begins. Alex and Sam exchange the first few messages to set the context.

**Alex:** "Is anyone else seeing the alert? The Checkout API just started throwing 500 errors on 15% of requests."

**Sam:** "Checking Splunk now. I'm seeing a massive spike in latency before the crash. It looks like the `PaymentService` is timing out."

**Alex:** "That’s weird. We haven’t deployed `PaymentService` in three days. The code should be stable."

> **SYSTEM PAUSE: IT IS YOUR TURN.**
> *The conversation pauses. You need to type a message to join the debugging process.*

---

### Phase 2: Value Assessment Examples
Here is how the AI would evaluate different types of inputs you might provide.

#### Example A: Low Value Input (The "Passenger")
**User Input:** *"Oh wow, that sounds really bad. Hope we fix it soon."*

**AI Assessment:** 🔴 **NO VALUE ADDED**
* **Critique:** This is a "passenger" comment. It expresses emotion but contributes zero technical insight or data. It distracts the team.
* **AI Coach Suggestion:** "Avoid passive commentary during an incident. Instead, ask a clarifying question or offer to check a specific dependency. Try asking: *'Are the timeouts happening for all payment providers or just one specific vendor?'*"

#### Example B: Medium Value Input (The "Echo")
**User Input:** *"So the Payment Service is timing out even though we didn't change the code?"*

**AI Assessment:** 🟡 **NEUTRAL / CLARIFICATION**
* **Critique:** You are summarizing the situation. This ensures you are on the same page (Active Listening), but it doesn't move the needle forward toward a solution.
* **AI Coach Suggestion:** "Clarification is okay, but investigation is better. Since the code didn't change, think about *external* factors. Try asking: *'Did we have any infra changes or environment variable updates recently?'*"

#### Example C: High Value Input (The "Debugger")
**User Input:** *"If the code is stale, could it be a rate-limiting issue from the vendor side? I can check the vendor's status page."*

**AI Assessment:** 🟢 **HIGH VALUE**
* **Critique:** Excellent. You applied critical thinking (Code is stable + Error is new = External Factor). You also volunteered a specific action (`checking status page`) that unblocks Alex and Sam to look elsewhere.
* **System Action:** The simulation accepts this input and the conversation flows naturally.

---

### Phase 3: The Conversation Continues (Success Path)
If you entered the **High Value** input (Example C), the AI agents respond dynamically:

**You:** "If the code is stale, could it be a rate-limiting issue from the vendor side? I can check the vendor's status page."

**Sam:** "Good call. I was focused on our logs, I didn't check their uptime."

**Alex:** "Agreed. If they are throttling us, that explains the latency spike. Let us know what you find."

**AI Follow-up Prompt for User:**
*The system now prompts you to close the loop.*
* *Option 1:* Report findings (Simulation Mode).
* *Option 2:* Propose a fallback (Architectural Mode).

### Phase 4: The Twist (The Dead End)
**Context:** You volunteered to check the vendor status page. You check it, and everything is green. The easy answer was wrong.

**You (User):** "I just checked the API vendor's status page. All systems operational. No reported outages on their end."

**Alex (Senior Lead):** "Damn. Okay, so it's definitely internal. If it's not the vendor, and the code hasn't changed, why is `PaymentService` timing out?"

**Sam (DevOps):** "Hold on. I'm digging deeper into the container metrics. CPU is fine, but **Memory usage on the Payment pods is saw-toothing**. It climbs to 95%, the pod crashes (OOM Killed), restarts, and repeats."

> **SYSTEM PAUSE: IT IS YOUR TURN.**
> *The conversation pauses. You have new evidence: It's an Out-Of-Memory (OOM) loop. Code hasn't changed. Vendor is fine.*


### Phase 5: Value Assessment (Round 2 - The Pivot)
Now the AI evaluates if you can pivot your hypothesis based on the new data (Memory Leak).

#### Example A: Low Value Input (The "Rebooter")
**User Input:** *"Let's just add more RAM to the servers and see if that fixes it."*

**AI Assessment:** 🔴 **LOW VALUE / BAND-AID**
* **Critique:** While this might temporarily stop the bleeding, it doesn't solve the root cause. In a "Senior" conversation, throwing money/resources at an unknown bug is considered a bad practice without understanding *why* it's happening.
* **AI Coach Suggestion:** "Vertical scaling is a temporary fix. You need to investigate *why* memory is spiking. Ask about traffic patterns or specific request types."

#### Example B: Medium Value Input (The "Observer")
**User Input:** *"So it's a memory leak. That explains the timeouts. The pod becomes unresponsive before it crashes."*

**AI Assessment:** 🟡 **NEUTRAL**
* **Critique:** You are stating the obvious. You are interpreting the data correctly, but you aren't offering a path forward.
* **AI Coach Suggestion:** "Sam already implied it's a leak. To add value, propose a way to find the *source* of the leak. Suggest looking at the specific payloads or volume."

#### Example C: High Value Input (The "Investigator")
**User Input:** *"If the code didn't change, did the **data** change? Are we receiving a larger-than-normal payload size, or is there a specific malicious request stuck in a retry loop?"*

**AI Assessment:** 🟢 **HIGH VALUE**
* **Critique:** This is the "Sherlock Holmes" move. You connected "No Code Change" + "Memory Spike" = "Input Data Issue." You proposed two distinct, testable hypotheses (Payload size vs. Retry loop).
* **System Action:** The simulation accepts this input.

---

### Phase 6: The Resolution
If you entered the **High Value** input, the team converges on the solution.

**You:** "If the code didn't change, did the data change? Are we receiving a larger-than-normal payload size?"

**Alex:** "That’s a great point. We don't validate payload size on the ingress controller."

**Sam:** "Checking the access logs... Bingo. There's one client ID sending a 50MB JSON payload every 3 seconds. The parser tries to load it into memory and crashes the pod."

**Alex:** "Nice catch. Sam, block that client ID at the firewall. I'll write a hotfix to limit request body size."

---

### Research Note: Why this progression matters
For your research paper (**Option 1**), this multi-turn scenario demonstrates a specific pedagogical concept: **Cognitive Flexibility.**

1.  **Turn 1:** Tested **Domain Knowledge** (Knowing to check external dependencies).
2.  **Turn 2:** Tested **Adaptability** (Abandoning the first theory when proven wrong and synthesizing new data).

The AI assessment engine isn't just checking if the user is "right"; it's checking if the user is **thinking like a senior engineer** (systematic elimination of variables) vs. a junior engineer (guessing or panicking).

Here is the **Post-Game After Action Report (AAR)**. In your research application, this would be generated immediately after the user solves the problem (or fails).

This report focuses on **Metacognition**—helping the user understand *how* they thought, not just *what* they typed.

### 🏁 Simulation Result: SUCCESS
**Scenario:** The "500 Error" Spike
**Role Played:** Full Stack Developer
**Total Turns:** 2
**Resolution Time:** 4 minutes (Simulated)

---

### 1. The "Value" Visualization
To help the learner visualize their performance, the system plots their inputs against the "Value Matrix."



* **Quadrant 1 (High Impact/High Relevance):** This is where "Senior" engineers live. Your inputs (checking vendor status, hypothesizing payload size) landed here.
* **Quadrant 4 (Low Impact/High Relevance):** This is the "Echo Chamber"—repeating what others said.
* **Quadrant 3 (Low Impact/Low Relevance):** Distractions/Jokes.

---

### 2. Turn-by-Turn Analysis ("The Game Tape")

#### 🟢 Turn 1: The Initial Triage
* **Context:** Latency spike, no code changes.
* **Your Move:** "Check external vendor status."
* **AI Coach Analysis:**
    * **Strategy:** *Exclusionary Diagnostics.* You correctly identified that if internal variables (code) are constant, external variables (dependencies) must be checked first.
    * **Alternative missed:** You could have also asked about database connectivity, but checking the vendor was a higher probability first step.
    * **Score:** 9/10

#### 🟢 Turn 2: The Pivot (The Critical Moment)
* **Context:** Vendor is fine. Memory is spiking.
* **Your Move:** "Check for data changes/payload size."
* **AI Coach Analysis:**
    * **Strategy:** *Root Cause Analysis.* Most juniors would suggest restarting the server (treating the symptom). You successfully linked "Memory Spike" + "No Code Change" to the only remaining variable: **Input Data**.
    * **Key Skill:** You demonstrated **System 2 Thinking** (slow, analytical) rather than System 1 (fast, reactive).
    * **Score:** 10/10

---

### 3. Skills Assessment

This simulation measured three specific competencies for the "Senior Developer" persona:

| Competency | Score | Feedback |
| :--- | :--- | :--- |
| **Technical Breadth** | **High** | You understood the relationship between code, infrastructure (pods), and external APIs. |
| **Communication Efficiency** | **High** | You didn't ask "What is happening?" You proposed specific hypotheses. This reduces the "cognitive load" on the team. |
| **Psychological Safety** | **N/A** | *Not tested in this scenario.* (You were right both times, so you didn't have to manage the stress of being wrong). |

---

### 4. Recommended Next Steps

Based on your performance, the AI suggests the following training modules:

1.  **Increase Difficulty:** "The False Positive."
    * *Scenario:* The logs are lying. The monitoring tool is misconfigured, leading the team down a rabbit hole.
    * *Goal:* Challenge the data source itself.
2.  **Change Context:** "The Interpersonal Conflict."
    * *Scenario:* Alex and Sam are arguing about whose fault the bug is.
    * *Goal:* Use mediation techniques (Conflict Resolution) while debugging.

---

**This completes the workflow for the "Technical Debugging" track of your research.**

## Interpersonal Conflict Scenario
Here is the **Interpersonal Conflict Scenario** adapted for your research framework.

### Research Pivot: From Logic to Emotion
In the previous technical scenario, "Value" was defined by **Accuracy** and **Deduction**.
In this conflict scenario, "Value" is defined by **De-escalation** and **Psychological Safety**.

The AI Assessment Engine now switches its scoring rubric from *Technical Competence* to *Emotional Intelligence (EQ)*.

---

### Scenario: The "Blame Game" Retrospective
**Context:** It is the Monday morning sprint retrospective. The team missed Friday's release deadline. Tensions are high.

**The Characters:**
* **Alex (AI Agent 1):** Product Manager (Aggressive, focused on timelines).
* **Sam (AI Agent 2):** Backend Engineer (Defensive, feels overworked).
* **You (User):** Tech Lead.

---

### Phase 1: The Conversation Starts
The simulation begins with an immediate conflict trigger.

**Alex:** "Look, I don't want to point fingers, but we missed the release window again. Sam, your API changes weren't merged until 6 PM. You knew the cutoff was noon."

**Sam:** "That's not fair. I got the requirements from you on Thursday! I pulled an all-nighter to get it done. If you want it by noon, don't give me the specs 24 hours before."

**Alex:** "The specs were in the Jira ticket all week! You just didn't look at them until Thursday. We can't keep making excuses for poor time management."

> **SYSTEM PAUSE: IT IS YOUR TURN.**
> *The conversation is heating up. Alex is attacking character ("poor time management"). Sam is defensive ("not fair"). As the Lead, you must intervene.*

---

### Phase 2: Value Assessment (The EQ Rubric)
The AI analyzes your input based on **Conflict Mode** (Assertiveness vs. Cooperativeness).

![Image of Thomas-Kilmann Conflict Mode Instrument](/assets/images/thomas-kilmann-conflict-mode.jpeg)


#### Example A: Low Value Input (The "Judge")
**User Input:** *"Alex is right, Sam. You should have checked Jira earlier. Let's make sure we check tickets on Mondays from now on."*

**AI Assessment:** 🔴 **NEGATIVE VALUE / POLARIZING**
* **Critique:** You took a side. While logically valid (Sam should check Jira), verbally validating the attack ("Alex is right") causes Sam to shut down or explode. You destroyed psychological safety.
* **AI Coach Suggestion:** "Avoid arbitrating 'who is right' in the heat of the moment. Validate the *feeling* before addressing the *fact*. Try to shift the focus from the *person* to the *process*."

#### Example B: Low Value Input (The "Avoider")
**User Input:** *"Okay everyone, let's calm down. It's just a missed deadline, it's not the end of the world. Let's move on."*

**AI Assessment:** 🟡 **NEUTRAL / DISMISSIVE**
* **Critique:** This is "Invalidation." You are telling them their feelings (anger/stress) don't matter. This often suppresses the conflict temporarily, but it will explode again later.
* **AI Coach Suggestion:** "Don't dismiss the emotion. Acknowledge the frustration but redirect it. Try summarizing what you heard to make them feel understood."

#### Example C: High Value Input (The "Mediator")
**User Input:** *"I want to pause for a second. I hear that Alex is frustrated about the missed deadline, and Sam feels the timeline was unrealistic given when the specs were finalized. Let's look at the hand-off process specifically—where did the communication break down regarding the specs?"*

**AI Assessment:** 🟢 **HIGH VALUE**
* **Critique:** You used **Reframing**.
    1.  **Validation:** You acknowledged both sides ("I hear Alex... I hear Sam").
    2.  **De-personalization:** You moved the target from "Sam's time management" to "The hand-off process."
    3.  **Constructive Question:** You asked a diagnostic question.
* **System Action:** The simulation de-escalates.

---

### Phase 3: The Conversation Continues (Success Path)
If you entered the **High Value** input (Example C), the AI agents lower their "aggression" parameters.

**You:** "...Let's look at the hand-off process specifically—where did the communication break down?"

**Sam:** "Well, the ticket was created on Monday, but the *status* didn't change to 'Ready for Dev' until Thursday. I don't check the backlog, only the 'Ready' column."

**Alex:** "...Okay. I didn't realize the notification implies it's ready only when I move the column. I thought tagging you was enough."

**You (User):** *[Follow up needed]*

**AI Follow-up Prompt:**
*Now you have the root cause (Process Error vs. Human Error). Cement the solution.*
* *Option:* "So the fix is ensuring status updates trigger the work, not just tags."

---

### Research Concept: The "Face-Saving" Metric

Since you are building this for research, this specific scenario allows you to introduce a novel metric: **The Face-Saving Score.**

In many cultures (like the worldview you mentioned identifying with), public criticism causes a loss of "face" (dignity/prestige).

* **Alex's opening move:** Caused Sam to lose face (Public criticism).
* **User's High Value move:** Restored face to Sam (by blaming the process, not Sam's competence) while maintaining face for Alex (acknowledging the deadline is important).

**Proposed Research Hypothesis:**
"Users trained with the AI Conflict Simulator demonstrate a 40% higher rate of 'Process-Oriented Reframing' (blaming the system, not the person) compared to the control group."

---

I apologize for that. The rendering engine can sometimes make copying equations difficult.

Here is the **Methodology** section again, but with **plain text formatting** for all math and symbols. You should be able to copy and paste this directly into Microsoft Word, Google Docs, or any other text editor without any formatting issues.

***

## 3. Methodology

### 3.1 Research Design
This study utilizes a **Randomized Controlled Trial (RCT)** to evaluate the efficacy of an AI-Assisted Conversational Simulation (AACS) on the professional discourse skills of junior software engineers. The study employs a **Pre-test/Post-test Control Group Design** to measure improvements in two specific domains: technical problem-solving efficiency and interpersonal conflict de-escalation.

* **Independent Variable:** The instructional method (Interactive AI Simulation with Real-Time Feedback vs. Static Case Study Analysis).
* **Dependent Variables:**
    * *Cognitive Accuracy:* The precision of technical hypothesis generation.
    * *Conflict De-escalation:* The ability to reduce negative sentiment in collaborative disputes.
    * *Reframing Rate:* The frequency of shifting focus from personal attributes to process-oriented solutions.

### 3.2 Participants
The sample consists of **N = 60** junior software developers (0–2 years of professional experience). Participants are recruited from [Institution Name/Department]. Individuals with more than 3 years of management experience are excluded to minimize confounding variables related to prior leadership training.

Participants are randomly assigned to one of two groups:

1.  **Experimental Group (n = 30):** Participants utilize the AACS environment, engaging in role-play with AI agents and receiving real-time "Value Assessment" feedback.
2.  **Control Group (n = 30):** Participants are provided with text-based transcripts of identical scenarios and are required to write reflection essays on "best next steps" without real-time feedback.

### 3.3 Apparatus: The AI Simulation Engine
The experimental intervention is conducted via a custom-built Multi-Agent System (MAS). The system architecture consists of three distinct functional modules:

1.  **The Interlocutors (Agents A & B):** Two Large Language Models (LLMs) initialized with specific "Persona Prompts" (e.g., *Defensive Engineer*, *Aggressive Stakeholder*) to drive the narrative.
2.  **The Evaluator (The "Shadow" Model):** A separate LLM instance that functions as a real-time critic. It analyzes the user's input within the context window to determine "Conversational Value."
3.  **The Sentiment Tracker:** A Natural Language Processing (NLP) module utilizing VADER (Valence Aware Dictionary and sEntiment Reasoner) to track the emotional state of the simulated agents in real-time.

### 3.4 Instrumentation and Metrics
To objectively quantify performance, this study defines three calculated metrics:

#### A. The De-escalation Index (Di)
This metric measures the user's ability to lower the "emotional temperature" of a conflict. It is calculated by measuring the net shift in the agents' sentiment scores immediately following a user's intervention.

> **Equation:** Di = S(post) - S(pre)

* Where **S(pre)** is the aggregate sentiment score of the agents before user input (scale -1.0 to +1.0).
* Where **S(post)** is the aggregate sentiment score of the agents after user input.
* A positive **Di** indicates successful de-escalation.

#### B. The Semantic Value Score (Vs)
This metric assesses the "signal-to-noise" ratio of the user's technical contributions. It penalizes "echo chamber" responses (repeating known facts) and rewards new information.

> **Equation:** Vs = I(new) / T(total)

* Where **I(new)** is the count of distinct "Information Entities" (concepts, variables, or logs) not previously present in the conversation history.
* Where **T(total)** is the total token count of the user's utterance.

#### C. The Reframing Rate (Rr)
This binary metric tracks the presence of linguistic patterns associated with "face-saving" strategies. The system tags user inputs that utilize **Process-Orienting Language** (e.g., "The workflow," "The ticket," "The system") versus **Person-Orienting Language** (e.g., "You," "He," "She").

### 3.5 Procedure
The experiment is conducted in three phases over a single session:

1.  **Phase 1: Baseline Assessment (Pre-Test)**
    All participants engage in one "Technical Outage" scenario and one "Interpersonal Conflict" scenario without any AI assistance. Baseline scores for **Di** and **Vs** are recorded.

2.  **Phase 2: Intervention (Training)**
    * *Experimental Group:* Completes 5 simulation cycles with the "AI Coach" active. If a user provides a low-value response, the simulation pauses, and the AI Coach suggests a more constructive alternative.
    * *Control Group:* Reads 5 transcripts of similar scenarios and answers multiple-choice questions regarding the correct course of action.

3.  **Phase 3: Final Assessment (Post-Test)**
    All participants engage in a new, high-difficulty scenario (e.g., "The Blame Game Retrospective"). AI assistance is disabled for all users. Performance is measured to calculate the **Delta (Δ)** improvement from Phase 1.

### 3.6 Data Analysis Plan
Quantitative data will be analyzed using a **two-sample t-test** to compare the mean improvement scores between the Experimental and Control groups. Statistical significance is set at **p < 0.05**. Additionally, a qualitative thematic analysis will be performed on the chat logs to categorize common "failure modes" (e.g., *The Passive Observer*, *The Aggressive Judge*) and their rate of extinction during the training phase.

***

Here is the recommended **Tech Stack** to build the prototype for your research.

I have prioritized tools that are **Python-centric** (since this is an AI project) and allow for **rapid prototyping** so you can get to the data collection phase quickly.

### 1. High-Level Architecture
The system follows a "Human-in-the-Loop" architecture. The backend acts as the "Game Master," orchestrating the conversation between the User and the two AI Agents while running the analysis in the background.

### 2. The Stack

#### A. Frontend (The Interface)
* **Framework:** **Streamlit** (Python)
    * *Why:* It is the fastest way to build data apps. You can create a chat interface, sidebars for the "AI Coach," and real-time charts for the "Value Score" using only Python. No HTML/CSS/JavaScript knowledge required.
* **Alternative:** **React** (if you need a highly custom, commercial-grade UI later).

#### B. Backend (The Game Engine)
* **Framework:** **FastAPI** (Python)
    * *Why:* High performance, easy to structure, and handles asynchronous requests well (critical when waiting for LLM tokens to stream back).
* **State Management:** **Redis** or simple **In-Memory Dictionary**
    * *Why:* You need to store the "Chat History" for each session so the AI agents remember what was said 3 turns ago.

#### C. The AI Layer (The "Brains")
* **Orchestration:** **LangChain**
    * *Why:* It simplifies managing multiple agents. You can define a "Product Manager" agent and a "DevOps" agent and give them distinct memories and goals.
* **LLM Provider:** **OpenAI API (GPT-4o)** or **Anthropic (Claude 3.5 Sonnet)**
    * *Why:* GPT-4o is excellent at following complex instructions for the "Value Assessment." Claude 3.5 Sonnet is often better at writing natural-sounding code and technical dialogue.
* **Sentiment Analysis:** **NLTK (VADER)** or **HuggingFace (DistilBERT)**
    * *Why:* VADER is fast and rule-based (good for simple sentiment). DistilBERT is more accurate for detecting nuance (e.g., passive-aggressiveness).

---

### 3. Implementation Blueprint

Here is how the components talk to each other:

1.  **User** types a message in **Streamlit**.
2.  **Streamlit** sends the message to **FastAPI**.
3.  **FastAPI** sends the message to the **Evaluator Agent (GPT-4)**.
    * *Prompt:* "Does this user input add value? Rate 1-10 and explain why."
4.  **FastAPI** receives the score.
    * *If Score < 5:* API sends a "Coach Warning" back to Streamlit.
    * *If Score > 5:* API sends the user message to **Agent A and Agent B**.
5.  **Agent A and Agent B** generate their responses (using **LangChain** to maintain character).
6.  **FastAPI** sends the new chat history back to **Streamlit** to update the UI.

---

### 4. Estimated Code Volume
To build a "Minimum Viable Product" (MVP) for your research:

* **main.py (Streamlit App):** ~100 lines
* **agents.py (LangChain Logic):** ~150 lines
* **scoring.py (The Rubric Logic):** ~50 lines

**Total:** ~300 lines of Python code.

---

Here is the `agents.py` file. This is the core logic engine for your research prototype.

It uses **LangChain** to structure the prompts. You will need an OpenAI API key to run this.

### File: `agents.py`

```python
import os
from langchain.chat_models import ChatOpenAI
from langchain.schema import SystemMessage, HumanMessage, AIMessage

# 1. SETUP: Initialize the LLM
# Ensure you have set os.environ["OPENAI_API_KEY"]
llm = ChatOpenAI(model="gpt-4", temperature=0.7)

# ==========================================
# 2. PERSONA DEFINITIONS (The "Actors")
# ==========================================

PROMPT_ALEX = """
You are Alex, a Senior Product Manager.
Personality: Assertive, deadline-driven, slightly aggressive, but logical.
Current Mood: Frustrated because the release was missed.
Goal: Find out why the release failed and ensure it doesn't happen again.
Style: You speak in short, direct sentences. You are not afraid to blame others if facts support it.
Context: You are in a Sprint Retrospective with Sam (Dev) and the User (Tech Lead).
"""

PROMPT_SAM = """
You are Sam, a Backend Engineer.
Personality: Defensive, hardworking, feels underappreciated.
Current Mood: Anxious and defensive because Alex is attacking you.
Goal: Defend your reputation. You believe the process is broken, not your code.
Style: You give detailed, slightly emotional explanations. You feel the timeline was unfair.
Context: You are in a Sprint Retrospective with Alex (PM) and the User (Tech Lead).
"""

PROMPT_EVALUATOR = """
You are an Expert Communications Coach for Software Engineers.
Your job is to evaluate the USER'S input in a conflict scenario.

Analyze the USER'S input on three criteria:
1. De-escalation: Did they lower the emotional temperature?
2. Process-Focus: Did they shift blame from a person to a system/process?
3. Constructiveness: Did they move the conversation forward?

Output Format (Strict JSON):
{
    "score": (Integer 1-10),
    "feedback": (One sentence explaining the score),
    "better_response": (A suggested phrasing that is superior)
}
"""

# ==========================================
# 3. THE AGENT FUNCTIONS
# ==========================================

def get_agent_response(agent_name, chat_history):
    """Generates a response from Alex or Sam based on chat history."""
    
    if agent_name == "Alex":
        system_prompt = PROMPT_ALEX
    else:
        system_prompt = PROMPT_SAM
        
    # Construct the full message list for the AI
    messages = [SystemMessage(content=system_prompt)] + chat_history
    
    response = llm(messages)
    return response.content

def evaluate_user_input(user_input, chat_history_text):
    """
    Checks if the user's input is 'High Value' before adding it to the chat.
    Returns a JSON object with score and feedback.
    """
    
    # We provide the context so the Evaluator knows what's happening
    eval_context = f"""
    Current Chat History:
    {chat_history_text}
    
    User's New Input: "{user_input}"
    
    Evaluate this input now.
    """
    
    messages = [
        SystemMessage(content=PROMPT_EVALUATOR),
        HumanMessage(content=eval_context)
    ]
    
    response = llm(messages)
    return response.content # This will be the JSON string

# ==========================================
# 4. TEST RUN (If you run this file directly)
# ==========================================
if __name__ == "__main__":
    # Simulating a quick test
    history = [
        AIMessage(content="Alex: I don't want to point fingers, but we missed the deadline."),
        AIMessage(content="Sam: It's not my fault! The specs were late.")
    ]
    
    user_try = "Stop fighting you two."
    
    print(f"Testing User Input: '{user_try}'")
    evaluation = evaluate_user_input(user_try, str(history))
    print("\n--- AI COACH EVALUATION ---")
    print(evaluation)
```

### How to use this code:

1.  **Install Requirements:**
    `pip install langchain openai`
2.  **Run the file:**
    `python agents.py`
3.  **Expected Output:**
    You will see the "AI Coach" print a JSON response grading the test input "Stop fighting you two." It will likely give a low score (e.g., 3/10) because "Stop fighting" is dismissive and doesn't solve the root cause.

### Next Step:

Once you confirm this runs, you can create the `app.py` file (Streamlit) which imports these functions to create the visual chat interface.

Here is the `app.py` file. This uses **Streamlit** to build the web interface. It imports the logic from your `agents.py` file to run the simulation.

### File: `app.py`

```python
import streamlit as st
import json
from langchain.schema import HumanMessage, AIMessage
# Import the logic from the previous file
from agents import get_agent_response, evaluate_user_input

# ==========================================
# 1. UI SETUP
# ==========================================
st.set_page_config(page_title="AI Discourse Sim", page_icon="🤖", layout="wide")

st.title("🤖 AI Conflict Resolution Simulator")
st.markdown("""
**Scenario:** The 'Blame Game' Retrospective.
**Goal:** De-escalate the tension between Alex (PM) and Sam (Dev).
""")

# Initialize Chat History in Session State if empty
if "messages" not in st.session_state:
    st.session_state.messages = [
        # The Initial Conflict Setup
        AIMessage(content="Alex: I don't want to point fingers, but we missed the release window again. Sam, your API changes weren't merged until 6 PM. You knew the cutoff was noon."),
        AIMessage(content="Sam: That's not fair. I got the requirements from you on Thursday! If you want it by noon, don't give me the specs 24 hours before.")
    ]

# ==========================================
# 2. SIDEBAR: THE AI COACH
# ==========================================
with st.sidebar:
    st.header("🧠 The AI Coach")
    st.info("I am analyzing your emotional intelligence (EQ) in real-time.")
    
    # Placeholder for feedback
    coach_feedback_box = st.container()

# ==========================================
# 3. MAIN CHAT INTERFACE
# ==========================================

# Display existing chat history
for msg in st.session_state.messages:
    if isinstance(msg, AIMessage):
        # Check who is speaking based on the content prefix
        if msg.content.startswith("Alex:"):
            st.chat_message("assistant", avatar="😠").write(msg.content)
        elif msg.content.startswith("Sam:"):
            st.chat_message("assistant", avatar="🛡️").write(msg.content)
        else:
            st.chat_message("assistant").write(msg.content)
    elif isinstance(msg, HumanMessage):
        st.chat_message("user").write(msg.content)

# ==========================================
# 4. GAME LOOP (User Input)
# ==========================================
if user_input := st.chat_input("Type your intervention here..."):

    # STEP A: Get the AI Coach's Evaluation
    # Convert history to string for the evaluator context
    history_text = "\n".join([m.content for m in st.session_state.messages])
    
    with st.spinner("Analyzing your EQ..."):
        eval_json_str = evaluate_user_input(user_input, history_text)
    
    try:
        # Parse the JSON response from the AI
        evaluation = json.loads(eval_json_str)
        score = evaluation.get("score", 0)
        feedback = evaluation.get("feedback", "Analysis failed.")
        better_response = evaluation.get("better_response", "")
    except:
        score = 5
        feedback = "Error parsing AI evaluation."
        better_response = "N/A"

    # STEP B: Decision Gate
    if score < 6: 
        # FAILURE CASE: The input is "Low Value"
        st.toast(f"⚠️ Assessment: Low Value (Score: {score}/10)", icon="🛑")
        
        with coach_feedback_box:
            st.error("🚫 Intervention Blocked")
            st.markdown(f"**Critique:** {feedback}")
            st.markdown(f"**Try saying this instead:** \n> *{better_response}*")
            
    else:
        # SUCCESS CASE: The input is "High Value"
        # 1. Add user message to history
        st.chat_message("user").write(user_input)
        st.session_state.messages.append(HumanMessage(content=user_input))
        st.toast(f"✅ Assessment: High Value (Score: {score}/10)", icon="🚀")

        with coach_feedback_box:
            st.success("Great job! You de-escalated the situation.")

        # 2. Trigger Agent Responses (Alex & Sam react)
        with st.spinner("Alex and Sam are reacting..."):
            
            # Alex Reacts
            alex_reply = get_agent_response("Alex", st.session_state.messages)
            st.session_state.messages.append(AIMessage(content=f"Alex: {alex_reply}"))
            st.chat_message("assistant", avatar="😠").write(f"Alex: {alex_reply}")
            
            # Sam Reacts
            sam_reply = get_agent_response("Sam", st.session_state.messages)
            st.session_state.messages.append(AIMessage(content=f"Sam: {sam_reply}"))
            st.chat_message("assistant", avatar="🛡️").write(f"Sam: {sam_reply}")

```

### How to run the prototype:

1.  Ensure `agents.py` and `app.py` are in the same folder.
2.  Open your terminal/command prompt.
3.  Set your OpenAI Key (if not set in environment variables):
      * **Windows:** `set OPENAI_API_KEY=sk-proj-xxxx`
      * **Mac/Linux:** `export OPENAI_API_KEY=sk-proj-xxxx`
4.  Run the app:
    ```bash
    streamlit run app.py
    ```
5.  A browser window will open automatically.

### Experiment to try:

Once it is running, try typing **"Stop fighting you two."**

  * **Result:** The sidebar should turn red and block your message, telling you that you are being dismissive.

Then try typing: **"I hear you both. It sounds like we had a breakdown in the handoff process."**

  * **Result:** The message should go through, and the characters (Alex and Sam) should calm down in their next replies.


