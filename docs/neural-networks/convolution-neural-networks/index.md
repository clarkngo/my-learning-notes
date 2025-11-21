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

