---
title: Day 1 Project Feedback
layout: default
parent: IKW
---

# 🎯 Day 1 Vibe Check: Target Audience & Critical Requirements

Feedback breakdown for the Day 1 vibe-coded mini-projects. For each project: who it's for, and the **one thing it has to do well** to actually work.

---

## 1. Nearby Journey — Travel Course Planner

* **Target Audience:** Travelers, tourists, and day-trippers looking for quick itinerary planning.
* **Must Do Well:** Map & Spatial Routing Integration — it needs to accurately map points of interest and generate a logically ordered, efficient route based on user inputs without sending users in circles.

## 2. Deep Sea Explorer

* **Target Audience:** Casual gamers looking for a quick arcade-style reflex challenge.
* **Must Do Well:** Collision Detection & Fluid Controls — movement must feel responsive, and the hitboxes for collecting coins versus hitting traps need to be fair and precise.

## 3. Top-Rated Restaurant Finder

* **Target Audience:** Diners and foodies wanting a quick, high-quality place to eat without digging through bad reviews.
* **Must Do Well:** Accurate Data Filtering — it must reliably filter out any establishment rated below 4.0 stars while returning up-to-date, relevant local results.

## 4. AI Truth Detection Game (First Version)

* **Target Audience:** AI enthusiasts, students, or curious players testing their ability to spot machine-generated text vs. human text.
* **Must Do Well:** Clear Analysis & Feedback — beyond just scoring right/wrong, it needs to provide insightful, accurate breakdowns of why a text sample was flagged as AI or human (e.g., highlighting syntax, tone, or perplexity indicators).

## 5. Eco Belt: Waste Sorter

* **Target Audience:** Casual gamers, students, or environmentally conscious users looking for a fast-paced sorting game.
* **Must Do Well:** State & Timer Synchronization — fast, click-based interactions must register instantly alongside an accurate, smooth countdown timer and real-time score tracking.

## 6. AI Truth Detection Game (Second Take)

* **Target Audience:** Casual quiz-goers looking for a quick challenge to spot real vs. synthetic content.
* **Must Do Well:** High-Quality Prompt/Text Contrast — the sample text pairs must be carefully curated so the challenge feels nuanced and realistic, rather than overly obvious or completely arbitrary.

## 7. Random Mock Investing

* **Target Audience:** Beginners, students, or casual users wanting to practice basic trading mechanics risk-free.
* **Must Do Well:** Portfolio & Math Logic — cash balance, buy/sell updates, dynamic price fluctuations, and total portfolio valuation calculations must remain 100% accurate and consistent across sessions.

## 8. Today's OOTD (Outfit-of-the-Day)

* **Target Audience:** College students and fashion-conscious individuals deciding what to wear daily.
* **Must Do Well:** Relevant Recommendation Logic — the recommendation engine must smartly translate questionnaire responses (e.g., weather, occasion, style preference) into coherent, practical outfit combinations.

## 9. Speed Clicker!

* **Target Audience:** Gamers wanting a fast reflex test or simple stress buster.
* **Must Do Well:** Latency & Click-Tracking Accuracy — high-frequency input handling needs to register every rapid click cleanly without lag, frame drops, or misclicks as the timer counts down.

## 10. Recycling Sorting Mini Game

* **Target Audience:** Casual gamers, students, or environmentally conscious users looking for a quick drag-and-drop sorting challenge.
* **Must Do Well:** State & Timer Synchronization — drag-and-drop drops must register instantly and score correctly (+10/-5) in sync with an accurate, uninterrupted countdown timer.

---

## 🔍 Build Review: Recycling Sorting Mini Game

Playtest notes on the actual build (screenshot review), checked against the **State & Timer Synchronization** requirement from item 10 above.

**What's working:**
* Score, countdown timer, and Restart are all visible in the header at once — the state the "must do well" requirement depends on is at least being tracked and surfaced together.
* The scoring rule is stated up front ("+10 if correct, -5 if wrong"), so the player knows the stakes before playing.
* General Waste as a catch-all last bin is a sensible design choice for items that don't cleanly fit the other four.

**What needs attention:**
* **No trash items are visible in the drop zone.** The whole game is "drag trash into the bin," but the play area is empty — either items aren't spawning, or they failed to render. This is the one thing to verify first, since it's the entire core loop.
* **Plastic vs. Vinyl is confusing.** The Vinyl bin's icon (a pump/lotion bottle) reads visually as plastic, and "Vinyl" isn't a bin category most players will recognize from real-world recycling. Two bins that look like they hold the same material invites wrong-bin drags that aren't really the player's fault — worth relabeling (e.g., "Glass" or merging into "Plastic") or picking a more distinct icon.
* **Can't verify the timer/score actually sync with drags** from a static screenshot — since there's nothing to drag yet, re-check that a countdown tick doesn't stall or skip while a drag is in progress, and that score updates land the instant a drop resolves (not on a delay).

---

## 📋 Summary Table

| # | Project | Target Audience | Must Do Well |
| --- | --- | --- | --- |
| 1 | Nearby Journey — Travel Course Planner | Travelers, tourists, day-trippers | Map & spatial routing integration |
| 2 | Deep Sea Explorer | Casual gamers wanting arcade reflex challenge | Collision detection & fluid controls |
| 3 | Top-Rated Restaurant Finder | Diners/foodies wanting quick, high-quality picks | Accurate data filtering (4.0+ stars, fresh results) |
| 4 | AI Truth Detection Game (v1) | AI enthusiasts, students, curious players | Clear analysis & feedback on AI vs. human text |
| 5 | Eco Belt: Waste Sorter | Casual gamers, students, eco-conscious users | State & timer synchronization |
| 6 | AI Truth Detection Game (v2) | Casual quiz-goers | High-quality prompt/text contrast |
| 7 | Random Mock Investing | Beginners/students practicing trading risk-free | Portfolio & math logic accuracy |
| 8 | Today's OOTD | College students, fashion-conscious individuals | Relevant recommendation logic |
| 9 | Speed Clicker! | Gamers wanting a reflex test/stress buster | Latency & click-tracking accuracy |
| 10 | Recycling Sorting Mini Game | Casual gamers, students, eco-conscious users | State & timer synchronization |
