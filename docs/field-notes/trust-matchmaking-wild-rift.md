---
title: "Trust, Matchmaking, and Mechanics: Inside Wild Rift's Development"
layout: default
parent: Field Notes
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

**Wild Rift Development & Community: Lessons Learned**

Based on the discussion with the Executive Producer of Wild Rift, several key lessons have been identified across gameplay, technical design, matchmaking, and community engagement.

Source: [A Discussion with the Executive Producer of Wild Rift](https://www.youtube.com/watch?v=PAX_CyevA90)

**1. Game Modes and Experimentation**
*   **Preserve the Core Experience:** The development team learned through the "T-Hex" experiment that too much deviation in the core Summoner's Rift mode can be frustrating for players who expect a familiar, structured experience (laning phases, skirmishes, and vision control). 
*   **Shift the Testing Grounds:** Moving forward, wild and crazy gameplay experiments will be channeled into alternate game modes (like ARAM or varied map modes). Only the most successful and well-received elements from these alternative modes will be considered for integration back into Summoner's Rift. 

**2. Champion Design and Technical Constraints**
*   **"Mobile-Native" is Non-Negotiable:** When designing "first-on-the-Rift" champions like Nora or porting PC champions, feeling native to mobile is an absolute requirement. The team realized that direct 1:1 PC ports do not always work; for example, complex champions like LeBlanc face unique mobile challenges regarding camera panning and aiming fidelity that must be solved creatively before release.
*   **Balance Fidelity with Performance:** Initially, Wild Rift was designed with extremely high-fidelity models meant to look good even if blown up on a 100-inch screen. However, this led to massive application sizes (currently around 14GB) and caused significant optimization issues, including device stuttering, battery drain, and overheating. The team learned they must carefully weigh the trade-offs between ultra-high graphics and actual mobile device constraints.

**3. Matchmaking and the Ranked Ladder**
*   **One System Cannot Serve All:** A single ranked ladder fundamentally fails to satisfy two entirely different player demographics: high-skill/low-engagement players and low-skill/high-engagement players. 
*   **Engagement Rewards Skew Matchmaking:** Rewarding players for sheer engagement (via Fortitude or loss-prevention systems) leads to rank inflation. This pushes players with negative win rates into higher ranks, ultimately resulting in low-quality matches for genuinely high-skill players. 
*   **Metrics Can Be Gamed:** The team saw that matchmaking algorithms factoring in personal stats (like KDA) were quickly exploited. Players intentionally ruined their stats (e.g., "Inting Sion") to trick the system into pairing them with highly skilled teammates to carry them. 
*   **Focus on Game Quality:** Rather than aiming strictly for 50/50 win-rate matches, the new matchmaking objective is to drastically increase the percentage of "high-quality games" where players feel the match was competitive and fair. Furthermore, validation for high skill must be separated from validation for high engagement.

**4. Esports and Organized Play**
*   **Avoid Competing with PC League:** Early attempts at a traditional, full-scale Wild Rift esports ecosystem (like Icons) struggled because the demand wasn't there; viewers preferred watching PC League of Legends esports.
*   **Pivot to Grassroots Community Play:** The future of organized play in Wild Rift lies in grassroots, community-focused, and creator-led tournaments. The team acknowledged a past failure in providing adequate tools for players and creators to easily organize these 5v5 local or guild-based events, which is now a priority moving forward.

**5. Communication and Trust**
*   **Silence Harms the Community:** The developer team learned that extended periods of silence regarding hot-button topics like matchmaking damage community trust. 
*   **Trust is the Core Currency:** Because the developers are asking for players' time and investment, maintaining a continuous dialogue and transparently addressing game flaws is essential to validating player efforts and maintaining the game's health. Furthermore, openly addressing optics—such as top leaderboard spots being occupied by rank-boosting services—is vital for the perceived integrity of the game.

# Four Quadrants of Players

During the discussion on matchmaking, the Executive Producer conceptualizes the player base using a "two-by-two" grid based on two specific metrics: **skill level** and **engagement** (how much time a player spends in the game). 

This creates four distinct categories or "quadrants" of players:

1. **Low Skill & Low Engagement:** Players who do not play often and have a lower skill level.
2. **High Skill & High Engagement:** Highly skilled players who dedicate a massive amount of time to the game.
3. **High Skill & Low Engagement:** Players who possess a high level of skill but rarely play the game.
4. **Low Skill & High Engagement:** Players who have a lower baseline skill level but invest a vast amount of time into playing.

**The Core Matchmaking Conflict**
The developers note that the system easily identifies and successfully manages the first two groups. The structural problems and complaints about matchmaking almost entirely stem from the system trying to handle the latter two groups (high skill/low engagement vs. low skill/high engagement). 

The perception of fairness is completely skewed between these two camps. High-skill players feel that their rank and match experience should reflect their ability, regardless of how often they play. Conversely, highly engaged players feel that their massive time investment should be rewarded, and they feel punished if their sheer effort isn't recognized by the system. 

**The Flaw of a Single Ranked Ladder**
The Executive Producer admits that **a single ranked ladder fundamentally fails because it is mathematically impossible for one system to satisfy both of these conflicting motivations at the same time**. Currently, the system tries to reward engagement through mechanics like "Fortitude," which inadvertently boosts highly engaged, lower-skilled players into higher ranks, completely breaking the competitive integrity and match quality for genuinely high-skilled players.

**The Developer's Solution**
Moving forward, the development team's strategy is to completely separate how these different groups receive validation for their efforts:
*   **Skill-Based Validation:** Hardcore "ladder chasers" need hyper-competitive environments (which the developers attempted with "Legendary Q") where rank is a strict reflection of skill, and reaching a specific leaderboard number is highly prestigious and earned.
*   **Engagement-Based Validation:** Players who play hundreds of games but aren't playing perfectly every match need separate avenues for reward that don't inflate the ranked ladder. This includes rewarding them for cool plays, experimentation, or achieving specific, localized goals like the Top 200 Champion leaderboards.

**The Conflict Between the Four Player Quadrants**
The core conflict within Wild Rift's matchmaking stems from trying to accommodate two fundamentally opposed groups on a single ranked ladder: **high-skill/low-engagement players** and **low-skill/high-engagement players**. High-skill players believe their match experience and rank should strictly reflect their actual ability. Conversely, highly engaged but lower-skilled players feel that their massive time investment should be recognized and rewarded, and they feel punished if they aren't progressing. The Executive Producer noted that a single ranked system mathematically cannot satisfy both of these conflicting expectations at the same time. 

**How 'Fortitude' Negatively Impacts High-Skill Matchmaking**
The Fortitude system, along with similar mechanics like "divine blessing" or loss prevention, inadvertently **rewards players for the sheer volume of games played rather than their actual skill level**. This creates a negative feedback loop: lower-skilled players (often with negative or sub-50% win rates) play poorly, get paired with highly skilled teammates to balance the match, and get carried. Because they play thousands of games, Fortitude prevents them from dropping and slowly boosts them into the highest ranks, such as the Apex or Sovereign tiers. As a result, genuinely high-skilled players are forced into highly unbalanced, low-quality matches because their teammates are in an ELO bracket they do not skillfully deserve. 

**Why Legendary Queue Failed to Satisfy the Competitive Community**
Legendary Queue was designed to be a hyper-competitive, high-stakes environment tailored specifically for hardcore "ladder chasers" to separate skill-based validation from engagement-based progression. While it succeeded on the Chinese server, the developers admit it was an **"execution error" in the rest of the world** because it failed to do what they expected. Ultimately, the mode struggled with low player participation, as the community noted that "no one really played it". Because the mode failed to catch on globally, Riot compensated by altering the standard ranked mode to try and appease everyone, which unfortunately perpetuated the issue of players climbing with negative win rates.

**Exploiting the KDA Matchmaking System**
High-skill players realized that the matchmaking algorithm heavily factored in personal performance stats, such as KDA, and began intentionally manipulating these metrics to get easier games. By purposely ruining their KDAs, they could trick the system into classifying them as lower-skilled players, which would prompt the algorithm to match them with highly skilled teammates—or "four gods"—to carry them. Players achieved this using strategies like "Inting Sion" or picking Leona, where they would ignore traditional skirmishes and simply push turrets or strictly farm thousands of Heartsteel stacks in the top lane. This allowed them to contribute to the team's win while keeping their own stats artificially deflated. Furthermore, these strategies were often used in tandem with systems like Galio's Aegis to ensure that even if they lost the match, their stats would protect them from losing ranked marks.

**Validation Tools for Engagement-Based Players**
To prevent engagement-based progression from inflating the competitive ranked ladder, Riot is actively working to separate how players receive validation. For highly engaged players who may not be playing perfectly in every match, the developers want to build separate avenues of recognition that reward them for making cool plays, experimenting with new builds, or trying out new mechanics. Existing localized goals, such as the Top 200 Champion leaderboards, have already proven to be a cool and successful way to reward players for the sheer effort and time they invest into specific characters. Additionally, features like the "adventure mode" are praised for effectively rewarding engagement, as players can continually gain points simply by playing a lot without the penalty of losing them. 

**Legendary Queue's Performance**
Legendary Queue was introduced as a hyper-competitive, high-stakes environment tailored for hardcore "ladder chasers". The mode succeeded on the Chinese server largely because China possesses the "majority of the player base," providing the necessary population to sustain it. However, the Executive Producer admits that its failure in the rest of the world was an "execution error" on Riot's part. In regions outside of China, Legendary Queue failed to do what the developers expected because it fundamentally lacked player participation, with the community observing that "no one really played it".

Riot is fundamentally changing its approach by separating skill-based validation from engagement-based validation, ensuring that time invested doesn't artificially inflate the competitive ranked ladder. To achieve this, they are focusing on several distinct validation tools for highly engaged players:

*   **Rewards for Localized Achievements:** Rather than tying all recognition to winning and losing, developers want to build separate avenues that **reward players for making cool plays, experimenting with new builds, or trying out new mechanics**. 
*   **Champion Leaderboards:** The existing system that ranks the **Top 1, Top 10, and Top 200 players for specific champions** is highlighted as a highly successful tool. It provides localized prestige and rewards players for the immense effort and time they dedicate to specific characters.
*   **Adventure Mode:** This feature is praised as an excellent vehicle for pure engagement validation because **players continually gain points simply by playing, without the penalty of losing points for match defeats**.
*   **Thematic Seasons as "Memory Markers":** Riot is utilizing massive thematic events—such as the "Ionia season" or the "T-Hex season"—to serve as historical markers. This strategy is designed to **validate the time and investment players put into the game by making them feel like they are participating in specific "chapters"** of Wild Rift's history.