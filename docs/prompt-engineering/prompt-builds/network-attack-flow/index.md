---
title: Animated Network Attack Flow — CIA Threat Simulator Component
layout: default
has_toc: false
parent: Prompt Builds
---

# In-Page Navigation
{: .no_toc }

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

```
CIA Threat Simulator — Reusable Component Prompt


Build a self-contained "CIA Threat Simulator" component in plain HTML/CSS/JS (no frameworks, no libraries). It must work as a drop-in <div> inside any dark-themed page.

─────────────────────────────────────────
OVERVIEW
─────────────────────────────────────────
An animated, scenario-driven attack visualizer that teaches the CIA Triad (Confidentiality, Integrity, Availability). The user selects an attack type from a dropdown and clicks ▶ Simulate. The UI runs a timed, multi-step animation showing the attack flowing through a network diagram, logging terminal output, and revealing which CIA pillar was violated.

─────────────────────────────────────────
VISUAL STRUCTURE — 4 STACKED LAYERS
─────────────────────────────────────────

Layer 1 — PHASE PROGRESS STRIP
A horizontal strip of 4 labelled phase pills (e.g. "Reconnaissance", "Vuln. Scanning", "Auth Bypass", "Data Exfiltration"). Phases light up left-to-right as the simulation progresses. Active phase = amber background. Completed phase = green background.

Layer 2 — SYSTEM FLOW DIAGRAM
A horizontal row of 4 nodes connected by lines:
  [🕵️ Attacker] ——›—— [Middle Node] ——›—— [🖥 Server] ——›—— [🗄 Target]
The middle node's icon and label changes per scenario (e.g. 🛡️ Firewall / 📡 Network / 🤖 Botnet). Nodes turn red and pulse when "hit" by the attack. Connector lines light red and animate glowing dots travelling from left to right to represent packets.

Layer 3 — LIVE TERMINAL LOG
A monospace scrolling terminal box (fixed height, overflow-y:auto) that receives timestamped log lines colour-coded as:
  - Red (tl-err): attack actions / breaches
  - Amber (tl-warn): warnings / anomalies
  - Green (tl-ok): defender events
  - Grey (tl-info): neutral info
Each log line slides in from below on append (CSS keyframe animation).

Layer 4 — CIA IMPACT INDICATORS
Three cards side-by-side: 🔒 Confidentiality / ✅ Integrity / ⚡ Availability.
At the end of the simulation, the violated pillar flips to a red "VIOLATED ✗" state (pulse flash animation). Safe pillars flip to green "Intact ✓".

─────────────────────────────────────────
CONTROLS
─────────────────────────────────────────
- <select> dropdown listing available scenarios
- ▶ Simulate button: disabled while animation runs, re-enabled on completion
- If no scenario selected, shake/highlight the dropdown in red for 1.2 s

─────────────────────────────────────────
DATA MODEL — THREAT_SCENARIOS OBJECT
─────────────────────────────────────────
Each scenario is a keyed entry with this shape:

const THREAT_SCENARIOS = {
  scenarioKey: {
    phases: ['Phase 1', 'Phase 2', 'Phase 3', 'Phase 4'],   // 4 phase labels
    mid: { i: '🛡️', l: 'Firewall' },   // middle-node icon + label
    srv: { i: '💻', l: 'App Server' }, // server-node icon + label
    tgt: { i: '🗄️', l: 'Database' },  // target-node icon + label
    steps: [
      // Each step fires at delay d (ms):
      { d: 0,    ph: 0, log: ['info', '> Starting recon...'] },
      { d: 700,  ph: 0, log: ['warn', '> Open ports detected'] },
      { d: 1400, ph: 1, log: ['err',  '> Exploiting...'], fc: 0 },
      // fc:0 fires a packet on connector 0 (Attacker→Mid)
      // fc:1 fires a packet on connector 1 (Mid→Server)
      // fc:2 fires a packet on connector 2 (Server→Target)
      // sn:'sn-mid' marks that node as hit (red pulse)
      // multi:true fires 4 rapid packets (flood effect)
      { d: 5600, ph: 3, log: ['err', '▓▓▓ BREACH COMPLETE ▓▓▓'], final: true },
    ],
    cia: { C: 'violated', I: 'safe', A: 'safe' },  // which pillar is hit
    ciaD: 5900,  // ms delay before CIA indicators flip
  }
};

// Built-in scenarios to include:
// breach    → Data Breach (SQLi → session hijack → DB dump) — C violated
// tampering → Data Tampering (ARP poison → MITM → modify transfer) — I violated
// ddos      → DDoS Attack (botnet flood → overload → 503) — A violated

─────────────────────────────────────────
KEY JAVASCRIPT PATTERNS
─────────────────────────────────────────

1. PACKET DOT ANIMATION — _firePacket(connId, multi)
   Creates a glowing red dot that slides from left:0 to left:calc(100% - 9px)
   on a position:absolute connector line. Uses DOUBLE requestAnimationFrame
   to guarantee the start position paints before the CSS transition begins
   (prevents the browser from skipping the start frame):

   function _firePacket(connId, multi) {
     const fc = document.getElementById(connId);
     fc.classList.add('fc-lit');
     const count = multi ? 4 : 1;
     const spd   = multi ? 380 : 540; // ms, faster for floods
     for (let j = 0; j < count; j++) {
       setTimeout(() => {
         const dot = document.createElement('div');
         dot.className = 'pkt-dot';
         dot.style.left = '0px';
         fc.appendChild(dot);
         requestAnimationFrame(() => requestAnimationFrame(() => {
           dot.style.transition = `left ${spd}ms linear`;
           dot.style.left = 'calc(100% - 9px)';
           setTimeout(() => dot.remove(), spd + 80);
         }));
       }, j * 140); // stagger multi-packets 140ms apart
     }
     setTimeout(() => fc.classList.remove('fc-lit'), spd + count * 140 + 100);
   }

2. TIMER CANCELLATION — _simTimers array
   Store every setTimeout ID in an array. On each new Simulate click,
   cancel all pending timers before starting fresh:

   let _simTimers = [];
   // At start of simulateThreat():
   _simTimers.forEach(t => clearTimeout(t));
   _simTimers = [];
   // When scheduling each step:
   _simTimers.push(setTimeout(() => { ... }, step.d));

3. PHASE PROGRESSION
   Track lastPh = -1. On each step, if step.ph !== lastPh:
   - Remove 'active' from previous phase pill, add 'done'
   - Add 'active' to current phase pill
   When final:true fires, remove 'active' from last pill and add 'done'.

4. LOG LINE APPEND
   Create a <div> with class "tl tl-err" (or tl-warn / tl-ok / tl-info),
   set its textContent, appendChild to log container, then:
   logEl.scrollTop = logEl.scrollHeight;  // auto-scroll to bottom

─────────────────────────────────────────
CSS CLASSES & ANIMATIONS
─────────────────────────────────────────

/* Phase strip */
.attack-phases { display:flex; border:1px solid #30363d; border-radius:6px; overflow:hidden }
.attack-phase  { flex:1; padding:.32rem; text-align:center; font-size:.69rem; background:#161b22; color:#8b949e; border-right:1px solid #30363d; transition:background .35s,color .35s }
.attack-phase.active { background:#2a1200; color:#f85149; font-weight:600 }
.attack-phase.done   { background:#162016; color:#3fb950 }

/* System flow */
.system-flow { display:flex; align-items:center; padding:.7rem; background:#0d1117; border:1px solid #30363d; border-radius:6px; gap:0 }
.sys-node    { text-align:center; padding:.35rem .5rem; border-radius:6px; background:#161b22; border:2px solid #30363d; min-width:60px; transition:border-color .4s,background .4s }
.sys-node.sn-hit { border-color:#f85149!important; background:#2a1c1c!important; animation:sn-pulse 1.2s ease-in-out infinite }
@keyframes sn-pulse { 0%,100%{box-shadow:0 0 0 0 rgba(248,81,73,.45)} 55%{box-shadow:0 0 0 7px rgba(248,81,73,0)} }

/* Connectors */
.flow-conn        { flex:1; height:2px; background:#30363d; position:relative; align-self:center; margin:0 3px; overflow:visible; transition:background .3s }
.flow-conn.fc-lit { background:#f85149 }
.flow-conn::after { content:'›'; position:absolute; right:-6px; top:-9px; color:#30363d; font-size:1rem; transition:color .3s }
.flow-conn.fc-lit::after { color:#f85149 }

/* Packet dot */
.pkt-dot { position:absolute; width:9px; height:9px; border-radius:50%; background:#f85149; top:-3.5px; left:0; pointer-events:none; box-shadow:0 0 5px #f85149 }

/* Terminal log */
.threat-log    { background:#0d1117; border:1px solid #30363d; border-radius:6px; padding:.5rem .75rem; font-family:monospace; font-size:.74rem; height:108px; overflow-y:auto; scroll-behavior:smooth }
.threat-log .tl { line-height:1.75; animation:tl-in .2s ease both }
.tl-err  { color:#f85149 }
.tl-warn { color:#e3b341 }
.tl-ok   { color:#3fb950 }
.tl-info { color:#8b949e; opacity:.85 }
@keyframes tl-in { from{opacity:0;transform:translateY(4px)} to{opacity:1;transform:none} }

/* CIA indicators */
.cia-impact-row  { display:grid; grid-template-columns:1fr 1fr 1fr; gap:.4rem }
.cia-impact-cell { text-align:center; padding:.4rem; border-radius:6px; background:#161b22; border:2px solid #30363d; font-size:.76rem; font-weight:600; color:#8b949e; transition:all .5s }
.cia-impact-cell.cic-violated { border-color:#f85149; background:#2a1c1c; color:#f85149; animation:cic-flash .5s }
.cia-impact-cell.cic-safe     { border-color:#3fb950; background:#1c2a1c; color:#3fb950 }
@keyframes cic-flash { 0%,100%{opacity:1} 40%{opacity:.4} }

─────────────────────────────────────────
CUSTOMISATION POINTS
─────────────────────────────────────────
- Add new scenarios: add a new key to THREAT_SCENARIOS with phases/steps/cia
- Change colour theme: swap #f85149 (red/attack) and #3fb950 (green/safe)
- Add a 5th node: add another .sys-node and .flow-conn to HTML, reference fc:3 in steps
- Extend log types: add tl-purple etc. with corresponding colour class
- Replay delay: ciaD controls how long after the last log line the CIA cards flip
- Flood mode: any step with multi:true fires 4 rapid overlapping packets
- Node relabelling: srv/tgt/mid icons and labels are set per-scenario at runtime

─────────────────────────────────────────
DARK THEME COLOR REFERENCE
─────────────────────────────────────────
Background:  #0d1117   Surface:    #161b22   Border:  #30363d
Text dim:    #8b949e   Text:       #c9d1d9   Heading: #e6edf3
Red (attack):#f85149   Amber:      #e3b341   Green:   #3fb950   Blue: #58a6ff
```