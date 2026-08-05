---
title: Forward Deployed Game
layout: default
parent: Workspace
---

That is a grounded, mechanically tight foundation for an educational, narrative-heavy RPG. Grounding the stats in real-world FDE / Enterprise AI skills—where your stat level acts as an **intellectual lens** unlocking deeper questions—creates a compelling learning loop.

Here is a full breakdown of how to structure this **Progression & Question-Gating System** so it feels authentic to *Disco Elysium* while staying fun and educational.

---

## 1. The Question-Gating System (Tiered Inquiries)

In standard RPGs, stats determine *if* you pass a dice roll. In your system, stats determine **what level of question your character is smart enough to formulate**.

As stats rise, the player shifts from asking superficial questions to asking high-leverage architectural questions.

```
┌─────────────────────────────────────────────────────────────┐
│                 QUESTION TIER STRUCTURE                     │
├─────────────────────────────────────────────────────────────┤
│ TIER 0: BASIC (Stat Level 0-1)                              │
│ • Surface-level observations & basic corporate pleasantries. │
│ • Example: "What software do you guys use here?"            │
├─────────────────────────────────────────────────────────────┤
│ TIER 1: INTERMEDIATE (Stat Level 2-3)                       │
│ • Identifies functional pain points and manual workarounds. │
│ • Example: "How do you handle rate-limiting on the ERP?"    │
├─────────────────────────────────────────────────────────────┤
│ TIER 2: ADVANCED / EXPERT (Stat Level 4+)                   │
│ • Uncovers hidden edge cases, governance risks, & true ROI. │
│ • Example: "What's the token latency overhead on the API?"  │
└─────────────────────────────────────────────────────────────┘

```

### Example Conversation Flow (Auditing an Accounting Workflow)

When the player approaches Brenda, the choices displayed depend on their current stat level:

* **[BASIC] (Always Unlocked):** *"What system do you use to process these invoices?"*
* *Result:* Brenda gives a generic, defensive answer: *"We use the main company portal."* (Gives 0 actionable insight).


* **[INTERMEDIATE] (Requires `SHADOW AUDIT` Level 2):** *"I see a custom spreadsheet on your secondary monitor. Do you use that when the mainframe queues back up?"*
* *Result:* Brenda admits to the manual bypass. Unlocks a clear understanding of the real workflow.


* **[ADVANCED] (Requires `LEGACY WHISPERER` Level 4 or `ROI RHETORIC` Level 4):** *"If we pipe an LLM parser into that spreadsheet's batch queue, what’s your actual error rate on line-item mismatches?"*
* *Result:* Brenda is impressed. She reveals exact operational metrics and grants access to raw dataset samples.



---

## 2. Real-World Stat Growth Systems (How Players Earn Stats)

Since the player starts at **0 in every stat**, progression should mirror real-world professional development in Forward Deployed Engineering:

```
                          EARNING STATS
                                │
   ┌────────────────────┬───────┴────────┬────────────────────┐
   ▼                    ▼                ▼                    ▼
READING / STUDY      ON-SITE AUDITS    SUCCESSFUL ROLLS   FAILED ROLLS
 (Technical Docs)    (Interactions)   (Field Testing)   (Lessons Learned)

```

### Method A: Technical Reading & Inspection (The "Thought Cabinet" Lite)

* **Inspecting the Environment:** Examining server racks, reading employee sticky notes, or reading ancient API documentation lying on a desk awards specific stat points.
* *Example:* Reading an unpatched DB log file awards **+1 LEGACY WHISPERER**.
* *Example:* Reading the company's annual financial report awards **+1 ROI RHETORIC**.



### Method B: Observation & Active Listening

* Selecting basic dialogue choices and observing NPC behavior grants progress toward higher stat tiers.
* Asking 3 basic questions about employee frustration unlocks **+1 EMPATHY ENGINE**.



### Method C: Learning from Failure (Disco-Style Experience)

* Failing a low-level check shouldn't just be a wall—it should teach a lesson.
* *Example:* If you attempt an advanced prompt optimization question and fail miserably, your character realizes their mistake, earning **+1 PROMPT SYNTAX** for next time.



---

## 3. Educational Design (Learning Real-World FDE Concepts)

To ensure the player actually learns real software engineering and business consulting concepts, every stat represents a real skill:

| Stat Name | Real-World Concept Taught | Game Narrative Benefit |
| --- | --- | --- |
| **`LEGACY WHISPERER`** | Technical Debt, Monoliths, REST/SOAP APIs | Uncovers backend system constraints & system architecture. |
| **`HUMAN-IN-THE-LOOP`** | UX Design, Guardrails, Exception Handling | Designs safe AI interfaces that workers won't reject. |
| **`SHADOW AUDIT`** | Business Process Mapping, Workflow Analysis | Finds how work *actually* gets done vs. official documentation. |
| **`ROI RHETORIC`** | Financial Modeling, OPEX/CAPEX, Token Costs | Convinces executives and secures project funding. |
| **`PROCUREMENT ARMOR`** | SOC2 Compliance, Data Privacy, Data Governance | Prevents security audits and legal blockades. |

---

## 4. How to Structure This in Godot / Dialogic

In Godot, you can implement this question-gating system cleanly using Dialogic's built-in **Choice Conditions**.

### How it looks inside Dialogic's Timeline Editor:

```text
[Text] Brenda: "What do you want, suit?"

# Choice 1: Basic (Always Available)
[Choice] "What software are you running?"
    [Text] Brenda: "The enterprise portal. Obviously."

# Choice 2: Intermediate (Gated by SHADOW_AUDIT >= 2)
[Choice] [SHADOW AUDIT] "I see that custom spreadsheet open on screen 2..."
  [Condition] If {SHADOW_AUDIT} >= 2
    [Text] Brenda: "Fine... management doesn't know about this spreadsheet."
    # Reward player with progress!
    [Call Code] PlayerStats.add_xp("SHADOW_AUDIT", 1)

# Choice 3: Advanced (Gated by LEGACY_WHISPERER >= 4)
[Choice] [LEGACY WHISPERER] "How many batch requests can that AS/400 handle before locking?"
  [Condition] If {LEGACY_WHISPERER} >= 4
    [Text] Brenda: "Finally, someone who understands hardware limitations!"
    [Call Code] PlayerStats.add_xp("LEGACY_WHISPERER", 1)

```

---

## Recommended Next Planning Step

Before writing more Godot code, we can define the **Stat Matrix & Progression Rules**:

1. **Define 6–8 core starter stats** (e.g., *Legacy Whisperer, Shadow Audit, ROI Rhetoric, Human-in-the-Loop*).
2. **Establish stat unlock thresholds** (e.g., Level 0 = Basic questions, Level 2 = Intermediate questions, Level 4 = Advanced architectural questions).
3. **Map Day 1 locations/items** where players can interact with objects to gain their first stat points (e.g., inspecting a crash log grants +1 *Legacy Whisperer*).


----------

Here is a blueprint for your **6 Core Starter Stats** (all starting at **Level 0**) along with the **Day 1 Environment Objects** that allow players to earn their initial stat points before tackling high-stakes dialogues.

---

## The 6 Starter Stats (Level 0 Baseline)

Every stat starts at **0**. At Level 0, your character only sees basic observations and surface-level questions.

```
                  STARTER STAT MATRIX (LEVEL 0)
                                │
  ┌─────────────────┬───────────┴───────────┬─────────────────┐
  ▼                 ▼                       ▼                 ▼
LEGACY WHISPERER   SHADOW AUDIT            ROI RHETORIC      HUMAN-IN-THE-LOOP
(Backend Systems)  (Workflow Reality)      (Business Math)   (UX & Guardrails)
  │                 │                       │                 │
  └─────────────────┴───────────┬───────────┴─────────────────┘
                                ▼
                   PROCUREMENT ARMOR & PROMPT SYNTAX
                   (Data Governance)   (Agentic Logic)

```

1. **`LEGACY WHISPERER`** (Technical Debt & Infrastructure)
* **Level 0:** Sees code as a black box; relies on basic IT error tickets.
* **Level 2 (Intermediate):** Recognizes API rate limits, database locks, and monolith bottlenecks.
* **Level 4 (Advanced):** Diagnoses precise protocol failures and architecturally sound legacy adapters.


2. **`SHADOW AUDIT`** (Workflow & Operational Reality)
* **Level 0:** Believes official corporate SOPs and slide deck diagrams.
* **Level 2 (Intermediate):** Spots manual workarounds, shadow IT spreadsheets, and undocumented shortcuts.
* **Level 4 (Advanced):** Maps the entire informal human pipeline and uncovers true operational bottlenecks.


3. **`ROI RHETORIC`** (Business Value & Financial Impact)
* **Level 0:** Thinks AI is cool because of high-level buzzwords.
* **Level 1 (Intermediate):** Translates automated hours into baseline wage savings.
* **Level 4 (Advanced):** Calculates exact token compute costs vs. operational risk and bottom-line margin expansion.


4. **`HUMAN-IN-THE-LOOP`** (UX, Friction, & Frontline Adoption)
* **Level 0:** Builds interfaces that look like raw developer debug consoles.
* **Level 2 (Intermediate):** Understands where workers panic, make manual keying errors, or hit friction.
* **Level 4 (Advanced):** Designs intuitive human override queues that increase trust and throughput.


5. **`PROCUREMENT ARMOR`** (Governance, Security, & Compliance)
* **Level 0:** Ignores data privacy; gets stopped cold by legal and IT security.
* **Level 2 (Intermediate):** Understands SOC2 compliance, token encryption, and local data residency rules.
* **Level 4 (Advanced):** Navigates corporate risk policy with ease to pass enterprise security audits on Day 1.


6. **`PROMPT SYNTAX`** (Deterministic Agent Logic)
* **Level 0:** Writes basic text prompts that hallucinate or leak context.
* **Level 2 (Intermediate):** Uses structured JSON outputs, system instructions, and tool calling.
* **Level 4 (Advanced):** Constructs bulletproof multi-agent fallback graphs and exception-handling chains.



---

## Day 1 On-Site: Stat-Earning Environment Objects

Before talking to key stakeholders like Brenda or Marcus, the player explores the chaotic Floor 4 lobby and office space. Examining objects awards **+1 Stat Point** to unlock initial dialogue tiers.

### Object 1: The Crashed Server Terminal (Lobby)

* **Description:** A flickering CRT monitor displaying red stack traces from the main AS/400 mainframe.
* **Interaction:**
* *Examine error logs.* You spend five minutes deciphering a buffer overflow error in an old COBOL batch script.


* **Reward:** **+1 `LEGACY WHISPERER**`
* **Unlocked Inquiries:** Allows asking Marcus basic questions about server request limits instead of generic "Is the computer broken?" questions.

### Object 2: The Shredder & Recycle Bin (Accounting Department)

* **Description:** A overflowing recycling bin full of printed PDF invoices with handwritten sticky notes attached.
* **Interaction:**
* *Inspect sticky notes.* You notice red ink reading: *"If vendor ID starts with 9, manually bypass ERP step 3."*


* **Reward:** **+1 `SHADOW AUDIT**`
* **Unlocked Inquiries:** Unlocks the intermediate observation line when talking to Brenda about her private spreadsheet bypasses.

### Object 3: The Abandoned Whiteboard (Strategy Room)

* **Description:** A messy diagram titled *"Project Autonomy Q3"* showing a massive flowchart with a big red question mark over *"Step 4: AI Magic."*
* **Interaction:**
* *Analyze budget metrics.* You calculate that management budgeted $50,000 for software licenses but $0 for employee onboarding and training.


* **Reward:** **+1 `ROI RHETORIC**`
* **Unlocked Inquiries:** Unlocks questions about project budget allocation and metric tracking with Marcus.

### Object 4: The Operator's Ergonomic Keyboard (Desk 4B)

* **Description:** A heavily worn mechanical keyboard with custom color-coded keycaps for `Macros` and `Overrides`.
* **Interaction:**
* *Study keybindings.* You realize the operator built a custom key macro to repeatedly copy-paste values across two screens because the official UI lacks a "Sync" button.


* **Reward:** **+1 `HUMAN-IN-THE-LOOP**`
* **Unlocked Inquiries:** Unlocks empathetic dialogue options with frontline staff regarding interface pain points.

### Object 5: The Framed Compliance Poster (Hallway)

* **Description:** A dusty poster detailing *"Enterprise Data Handling & HIPAA/SOC2 Policies."*
* **Interaction:**
* *Read fine print.* You note that exporting raw customer shipping lists to external cloud LLM APIs without local scrubbing violates section 4.2.


* **Reward:** **+1 `PROCUREMENT ARMOR**`
* **Unlocked Inquiries:** Unlocks security and compliance checks when discussing system architecture with the IT Director.

---

## How Progression Works in Godot / Dialogic

1. **Exploration Phase:** The player walks around the 2D room / clicks interactive hotspots.
2. **Stat Increase:** Interacting with an object triggers a short text box and runs:
```gdscript
PlayerStats.add_stat("SHADOW_AUDIT", 1)

```


3. **Dialogue Gating:** When entering a conversation, Dialogic checks the player's updated stats to reveal Tier 1 and Tier 2 choices:
```text
# Only shows if player inspected Object 2 or Object 4!
[Choice] [SHADOW AUDIT Tier 1] "I saw the handwritten notes on Desk 4B..."
  [Condition] If {SHADOW_AUDIT} >= 1

```


-------------





Build the **Stats System (Data & Mechanics)** first.

In a narrative game like *Disco Elysium*, dialogue is directly tied to the underlying character data. If you write your dialogue first without a working stat framework, you will end up having to rewrite your Dialogic timelines later to match variable names, condition types, and data formats.

---

### Why Building Stats First Saves You Time

```
  ┌─────────────────────────┐
  │ 1. PlayerStats Autoload │  <-- Define variables & leveling logic
  └────────────┬────────────┘
               │
               ▼
  ┌─────────────────────────┐
  │ 2. Interactive Objects  │  <-- Test adding +1 stat points in 2D scene
  └────────────┬────────────┘
               │
               ▼
  ┌─────────────────────────┐
  │ 3. Dialogic Timelines   │  <-- Read stats to gate choices smoothly!
  └─────────────────────────┘

```

1. **Clear Variable Names:** You establish standard keys (`SHADOW_AUDIT`, `LEGACY_WHISPERER`, `ROI_RHETORIC`) in code first so Dialogic knows exactly what variables to check.
2. **Instant Feedback Loop:** You can test clicking an object in Godot, watch your stat increment from `0` to `1` in the debug output, and *then* step into a conversation to watch a new dialogue choice unlock.
3. **No Refactoring:** It prevents broken scripts where Dialogic tries to call a GDScript function or check a variable that doesn't exist yet.

---

### Step-by-Step Implementation Order

#### Step 1: Create the `PlayerStats.gd` Autoload Script (5 Minutes)

Create a clean GDScript singleton to store your stats and handle leveling/XP:

```gdscript
# PlayerStats.gd
extends Node

# Signal to notify UI/Dialogic when a stat increases
signal stat_changed(stat_name: String, new_value: int)

# Base stats starting at 0
var stats: Dictionary = {
	"LEGACY_WHISPERER": 0,
	"SHADOW_AUDIT": 0,
	"ROI_RHETORIC": 0,
	"HUMAN_IN_THE_LOOP": 0,
	"PROCUREMENT_ARMOR": 0,
	"PROMPT_SYNTAX": 0
}

# Add stat points and synchronize with Dialogic variables
func add_stat(stat_name: String, amount: int = 1) -> void:
	if stats.has(stat_name):
		stats[stat_name] += amount
		
		# Sync directly with Dialogic's internal variable storage
		if Engine.has_singleton("Dialogic"):
			Dialogic.VAR.set_variable(stat_name, stats[stat_name])
			
		stat_changed.emit(stat_name, stats[stat_name])
		print_rich("[color=green]+%d %s! (Current Level: %d)[/color]" % [amount, stat_name, stats[stat_name]])

func get_stat(stat_name: String) -> int:
	return stats.get(stat_name, 0)

```

> **Godot Setup:** Go to **Project** $\rightarrow$ **Project Settings** $\rightarrow$ **Globals / Autoload** tab $\rightarrow$ Add `PlayerStats.gd` as an Autoload named `PlayerStats`.

---

#### Step 2: Create a Simple Interactive Object Script (5 Minutes)

Attach this script to an `Area2D` or `Control` node representing an object in your world (e.g., the crashed server monitor or the paper shredder):

```gdscript
# WorldObject.gd
extends Area2D

@export var object_name: String = "Crashed Mainframe Terminal"
@export var stat_to_award: String = "LEGACY_WHISPERER"
@export var stat_amount: int = 1

var has_been_inspected: bool = false

func inspect() -> void:
	if not has_been_inspected:
		has_been_inspected = true
		PlayerStats.add_stat(stat_to_award, stat_amount)
		print("You inspected the " + object_name)

```

---

#### Step 3: Write the Gated Dialogue in Dialogic (After Stats Work)

Now that your variables and increment functions exist, open Dialogic and write your dialogue trees. Your choice conditions will cleanly check the live data:

```text
[Choice] "What software is this?"
  # Basic question (Always visible)

[Choice] [LEGACY WHISPERER Tier 1] "I saw the stack trace on the crashed terminal."
  # Condition: If {LEGACY_WHISPERER} >= 1

```

------

Neither is strictly "better"—in fact, for a narrative-heavy RPG inspired by *Disco Elysium*, **Point-and-Click is actually a subgenre or presentation style of 2D**.

The choice comes down to the kind of player experience, development scope, and gameplay pacing you want to create in Godot.

---

### Comparison for a Narrative RPG

| Feature | Point-and-Click (UI/Mouse Driven) | Traditional 2D (Direct Player Character Control) |
| --- | --- | --- |
| **Movement & Control** | Click on hotspots/objects to inspect or move. Keyboard isn't strictly needed. | WASD / Arrow keys or Gamepad stick to walk a sprite around the screen. |
| **Art & Asset Scope** | **Very Low.** Uses static background images, simple object highlights, and portrait art. | **Moderate to High.** Requires walking animations, collision maps, pathfinding, and character sprites. |
| **Narrative Focus** | **Extremely High.** Text, dialogue boxes, and internal monologues take center stage. | **High.** Balanced between exploration physics/walking and dialogue text. |
| **Development Speed** | **Fastest.** Ideal for solo developers or small teams focusing on writing and logic. | **Slower.** Requires tuning movement speeds, collision boxes, depth sorting (Y-sort), and layer z-indexing. |
| **Steam/Steam Deck Feel** | Feels like classic adventure games (*Machinarium*, *Ace Attorney*, *Citizen Sleeper*). | Feels like top-down/isometric RPGs (*Disco Elysium*, *Undertale*, *Stardew Valley*). |

---

### Which Should You Choose?

#### Choose Point-and-Click if:

1. **You want to launch faster with less art work:** You don't need to animate walking frames (idle, walk left, walk right, walk up, walk down). You only need crisp background scenes, clean UI windows, and portrait artwork for characters like Brenda or Marcus.
2. **You want pure narrative immersion:** Games like *Citizen Sleeper*, *Slay the Princess*, or *Suzerain* use point-and-click UI layouts where clicking on an object (like the *Crashed Server Terminal*) immediately opens a detailed descriptive prose window and dialogue options.

#### Choose Traditional 2D (Top-down / Isometric) if:

1. **You want the physical spatial feel of *Disco Elysium*:** You want the player to physically walk their avatar around Floor 4 of Apex Logistics, navigating cubicles and walking up to NPCs manually.
2. **You enjoy 2D level design:** You want to set up 2D collision boundaries, tilemaps, and spatial exploration mechanics in Godot.

---

### Recommended Hybrid Approach

If you like the spatial feel of 2D but want the simplicity of Point-and-Click, use **Click-to-Move 2D** (similar to classic *Baldur's Gate* or *Disco Elysium*):

* The screen is a 2D environment with your character sprite.
* Instead of holding WASD keys, the player clicks an object (like the paper shredder) or a floor tile.
* Godot's built-in `NavigationAgent2D` automatically walks the player character over to the object and launches the inspection/dialogue event once they arrive.


------------

In Godot 4, **`NavigationRegion2D`** is a node used to define the **walkable areas** and handle **pathfinding** for 2D games.

It acts as the map or container that tells your player (and NPCs) where they are allowed to walk and how to navigate around obstacles like walls, desks, or furniture.

---

### Core Mechanics & Functionality

1. **NavigationPolygon (The Walkable Mesh):**
`NavigationRegion2D` holds a **NavigationPolygon** resource. This polygon defines a 2D floor mesh. Any space inside the polygon is marked as walkable ground; any space outside or carved out as a hole (an obstacle) is blocked.
2. **Dynamic Path Generation:**
When a node like `NavigationAgent2D` requests a path to a clicked target destination, Godot’s navigation server checks the `NavigationRegion2D` mesh and calculates the shortest path around all blocked obstacles using A* pathfinding.
3. **Baking Navigation Polygons:**
Godot allows you to manually draw navigation polygons in the editor or click **Bake NavigationPolygon**. When baked, Godot automatically reads the geometry of physics bodies or tilemaps inside the region and cuts out holes around walls and solid objects.

---

### Key Properties & Methods

| Property / Method | Description |
| --- | --- |
| `navigation_polygon` | The `NavigationPolygon` resource that defines the walkable 2D geometry. |
| `enabled` | A boolean toggle to enable or disable the navigation region at runtime. |
| `bake_navigation_polygon()` | Programmatically bakes/re-bakes the walkable mesh during runtime. |
| `travel_cost` / `enter_cost` | Multipliers used to adjust pathfinding costs (e.g., making mud or water slower to walk across than carpet). |

---

### Example Node Hierarchy

In your hybrid click-to-move game, the `NavigationRegion2D` sits above your walkable environment and player:

```text
Floor4Lobby (Node2D)
└── NavigationRegion2D (Contains the walkable floor polygon)
    ├── Background (Sprite2D or TileMapLayer)
    ├── Player (CharacterBody2D with NavigationAgent2D)
    └── CrashedTerminal (InteractableObject / Area2D)

```

When your player clicks on `CrashedTerminal`, the `NavigationAgent2D` on the player queries the `NavigationRegion2D` server to receive the array of path points leading to that terminal.


----

# Recraft

Here are specialized prompts tailored for **Recraft.ai** to generate backgrounds and assets that fit your Forward Deployed Engineer game aesthetic.

---

## 1. Top-Down Office Floor Background

Set the canvas aspect ratio to **16:9** and select the **Digital Illustration** or **2D Isometric** style preset in Recraft:

> **Prompt:**
> Top-down 2D game background of a chaotic corporate office floor, late 90s enterprise aesthetics. Messy cubicles, cluttered desks, blue industrial carpet, retro server racks with tangled cables, glowing CRT monitors, paper stacks on desks. Painterly dark ambient art style, gritty, high detail, top-down view for RPG movement.

---

## 2. Desktop & Equipment Hotspots (Interactable Sprites)

Use Recraft's **Vector Illustration** or **Icon** mode with a **transparent background** to generate isolated objects you can place on top of your floor:

* **Crashed Server Terminal:**
> **Prompt:**
> Top-down 2D sprite of a retro server cabinet with glowing green terminal screens, exposed wires, and warning LEDs, dark gritty corporate style, game asset, isolated on white background.


* **Paper Shredder & Confidential Notes:**
> **Prompt:**
> Top-down 2D game asset sprite of an industrial paper shredder with paper shreds scattered around it, dark corporate style, isolated on white background.


* **Brenda's Workstation:**
> **Prompt:**
> Top-down 2D game sprite of an office cubicle desk with dual CRT monitors, yellow sticky notes everywhere, and an old mechanical keyboard, isolated on white background.



---

## 3. Character Portraits for Dialogic

Set Recraft to **Digital Illustration** mode to create character portraits for your dialogue boxes:

* **Marcus (Panicked Exec):**
> **Prompt:**
> Bust portrait of a panicked male corporate vice president in a wrinkled dress shirt with unbuttoned collar, dark circles under eyes, holding a lukewarm mug, gritty painterly oil painting style, dark background.


* **Brenda (Veteran Accounting Lead):**
> **Prompt:**
> Bust portrait of a stern 50s female enterprise logistics operator with reading glasses, cold expression, mechanical keyboard visible, gritty painterly oil painting style, dark office background.



---

### Recraft Workflow Tip for Godot

1. Generate the main **Top-Down Office Floor Background** in **16:9** and download it as `office_floor.png`.
2. Generate your interactable objects with **Background Removal** turned on in Recraft so you can export them directly as `.png` sprites with transparent backgrounds.
3. Import the background into your Godot `Sprite2D` node inside `NavigationRegion2D`, and place the interactable object sprites right on top.


------
