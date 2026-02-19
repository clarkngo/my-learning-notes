---
title: Trust, Matchmaking, and Mechanics: Inside Wild Rift's Development
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
