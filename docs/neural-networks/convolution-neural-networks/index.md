---
title: Convolutional Neural Networks
layout: default
parent: Neural Networks
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Visualized Explanation
- [Convolutional Neural Networks Explained (CNN Visualized)](https://youtu.be/pj9-rr1wDhM?si=egpbExOugTcBTrbr)
- [What Are Word Embeddings?](https://youtu.be/hVM8qGRTaOA?si=RaqrH3VvFpwulCvq)

# Notes
- 3x3 can be used for HD images but your focus is a small feature. i.e. traffic light
- filters going for 32->64->64->128. this is increasing because the permutations of different nodes. If its the other way, it will losing information of the features.
- paddings in CNN is like putting a picture frame before you start cutting up your image. You will ran out of picture! Padding will add extra space around the edges of the picture before you start looking at it closely. Keeps the picture the right size to analyze all the details.
- pooling is reducing with the max values in a slice. It is downsampling.

## Analogy
Here is an analogy using a **Corporate Chain of Command** to explain the hierarchy and the feed-forward process.

Imagine a massive company whose only job is to look at satellite photos and decide: **"Is this a stadium?"**

The company is organized into strict layers of employees.

### 1. The Interns (The First Layer - Edges)
At the bottom, you have thousands of interns. They are not allowed to see the whole photo. Each intern is given a magnifying glass and told to look at one tiny 5x5 square of the photo.
* **Their Job:** They only report simple things.
* **The Report:** "I see a curved line," or "I see a patch of green," or "I see a sharp corner."
* **Feed Forward:** They pass their sticky notes up to the Managers. They do not talk to each other.

### 2. The Middle Managers (The Middle Layer - Shapes)
The managers do not look at the photo at all. They only look at the sticky notes from the interns.
* **Their Job:** They look for patterns in the intern reports.
* **The Logic:** A manager notices that Intern A saw a "curve," Intern B saw a "curve," and Intern C saw a "curve."
* **The Report:** The manager combines these and reports: "There is a **Circle** in sector 4." Another manager reports: "There is a **Rectangle** in sector 5."
* **Feed Forward:** They pass their reports up to the Directors.

### 3. The Directors (The High Layer - Objects)
The Directors don't look at the photo, and they don't care about the interns' lines. They only read the Managers' reports about shapes.
* **Their Job:** Assemble shapes into objects.
* **The Logic:** The Director reads: "We have a massive Oval (the stands) surrounding a green Rectangle (the field)."
* **The Report:** "This looks like a **Sports Complex**."

### 4. The CEO (The Output Layer - Classification)
The CEO sits at the top. They receive the final report from the Directors.
* **The Decision:** Based on the report of a "Sports Complex," the CEO stamps the file: **"STADIUM: 99% Confidence."**

***

### Why this analogy fits the "Feed Forward" concept:

1.  **Abstraction:** The CEO (Output) has no idea what the original pixels looked like. They only know the high-level concept. The information became more abstract as it moved up the chain.
2.  **One-Way Flow:** The CEO never runs down to the basement to ask an intern, "Are you sure that line was curved?" The information only flows upward.
3.  **Localized Vision:** The interns (Convolution Filters) only see a tiny piece of the world. It takes the hierarchy to understand the big picture.

# AI-Unplugged for CNN: Limited Vision (Receptive Fields) and Hierarchical Construction

This is a classic "Unplugged" activity often used to teach Computer Science concepts. We will call this game **"The pixel Assembly Line."**

It gamifies the specific constraints of a CNN: **Limited Vision (Receptive Fields)** and **Hierarchical Construction.**

### Game Title: The Pixel Assembly Line

**Objective:** The team must identify a mystery image, but no one person is allowed to see the whole image. They must rely on the "Feed Forward" process.

**Players:** 6–10 people (Scalable).
**Materials:**
* 1 Large printout of a simple object (e.g., a House, a Smiley Face, a Car, or a Cat).
* 1 large piece of opaque paper (to cover the printout) with a 3x3 grid cut out of it (or just cut the original image into 9 squares).
* Small index cards or sticky notes.
* Pens/Markers.

---

### The Setup (The Hierarchy)
Arrange your players in three distinct rows (Layers).

**Row 1: The Scanners (The Filters)**
* **Number of Players:** 3-4 (Assign each player specific "grid squares" of the image).
* **Constraint:** They are **only** allowed to see their specific square. They cannot look at their neighbor's square.
* **Job:** They look at the pixels and draw the *lines* they see on an index card.
* **Strict Rule:** They cannot name shapes. They can only draw lines (vertical, horizontal, diagonal, curved).

**Row 2: The Builders (The Feature Map)**
* **Number of Players:** 2-3.
* **Constraint:** They cannot see the original image *at all*. They can only look at the index cards passed to them by Row 1.
* **Job:** They take the index cards from Row 1, tape them together, and try to identify a **Shape** or **Part**.
* **Strict Rule:** They interpret the lines. They write down "I see a Triangle," "I see a Circle," or "I see Whiskers."

**Row 3: The AI (The Output Layer)**
* **Number of Players:** 1.
* **Constraint:** Can only read the words written by Row 2.
* **Job:** Guess the object.

---

### The Gameplay (The Feed Forward)

**Round 1: The Input**
The Facilitator (You) walks to **Row 1**. You show Player A *only* the top-left corner of the mystery image (e.g., the point of a roof).
* *Player A thinks:* "I see two diagonal lines meeting."
* *Action:* Player A draws an inverted 'V' on a card and passes it to Row 2.

You show Player B the middle square (e.g., a window).
* *Player B thinks:* "I see four perpendicular lines."
* *Action:* Player B draws a hashtag/square shape and passes it to Row 2.

**Round 2: The Feature Extraction**
**Row 2** receives these cards. They don't know where they came from contextually, but they analyze the pattern.
* *Builder 1 says:* "Okay, I have a pointy angle here. That looks like a **Triangle**." (Writes "Triangle" on a card).
* *Builder 2 says:* "I have a box shape with a cross. That looks like a **Square** or a **Window**." (Writes "Square/Window" on a card).
* *Action:* They pass these text cards to Row 3.

**Round 3: The Classification**
**Row 3 (The AI)** receives the cards: "Triangle" and "Square/Window."
* *The AI thinks:* "What object has a triangle on top of a square?"
* *Action:* The AI shouts: **"It's a House!"**

---

### The Educational Debrief (The "Why")

After the game (whether they win or lose), gather everyone and explain the parallels:

1.  **To Row 1 (The Scanners):** "Did you know you were drawing a house?"
    * *They will say No.*
    * **Lesson:** This is the **Convolutional Layer**. It doesn't understand objects; it only understands edges and contrast. It has a small 'Receptive Field.'

2.  **To Row 2 (The Builders):** "Could you have guessed it was a house without Row 1?"
    * *They will say No.*
    * **Lesson:** This is the **Hidden Layer**. It aggregates simple features into complex shapes. It needs the data from the previous layer to function.

3.  **To Row 3 (The AI):** "Why didn't you just look at the drawing?"
    * **Lesson:** Computers can't "see." They only process math and probability. You represented the **Fully Connected Layer** that takes high-level data and assigns a label.

4.  **Failure Mode (Optional):** If the game fails (e.g., they guess "Arrow" instead of "House"), explain that this is **Loss**. In a real AI, we would send a signal back (Backpropagation) telling Row 2 and Row 1 to adjust how they interpret the lines next time.

### Visual Aid for the Room
Draw this on the whiteboard to track progress:

`[ Pixels ] --> [ Lines ] --> [ Shapes ] --> [ Object ]`