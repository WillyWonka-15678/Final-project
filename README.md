# Fogg's Ant Farm


> A tabletop mixed-reality installation critiquing BJ Fogg's Behavior Model (B = MAP) and the gamification of human labour.

**MA Computational Arts — Goldsmiths, University of London — 2026**

---
## Overview

*Fogg's Ant Farm* is a single-participant mixed-reality installation. An acrylic display case lined with black velvet sits on a wooden table. Inside: two colour-coded wooden blocks. Wearing a Meta Quest 3 headset, the participant uses their hands to place blocks into the case, intervening in the performance metrics of two miniature virtual workers overlaid via MR passthrough.

A floating **God Dashboard** monitors the workers in real time. Its second column — revealed only after the participant's eighth block placement — monitors the participant themselves, formatted in identical visual language.

The work takes its title from **BJ Fogg**, whose Behavior Model reduces human action to a single formula:

```
B = M × A × P
(Behavior = Motivation × Ability × Prompt)
```

This formula now underlies the design of gig economy platforms, productivity tools, and social media feeds. The installation makes it physical, embodied, and implicating.

---
## Hardware Requirements

| Item | Spec |
|---|---|
| Headset | Meta Quest 3 |
| Computer | Windows laptop, Unity-capable GPU |
| Table | Standard wooden table |
| Case | Acrylic display case, 30 × 20 × 10 cm |
| Lining | Black velvet |
| Blocks | 2 wooden blocks, 3 × 3 × 3 cm, colour-coded |
| Lighting | Directed overhead spot light, 4000K |

---

## Software Stack

| Layer | Technology |
|---|---|
| Engine | Unity 2022 LTS, URP |
| MR Runtime | Meta XR SDK + Quest 3 Passthrough |
| Block Tracking | Unity AR Foundation — Image Tracking |
| Spatial Anchor | Manual 3-point hand-gesture calibration |
| Characters | Quaternius Universal Animation Library (CC0) |
| UI | World Space Canvas + TextMeshPro |

---
## How It Works

### Block Types

| Block | Colour | Label | Effect on workers | Hidden system effect |
|---|---|---|---|---|
| Block 1 | Deep red | BONUS | Worker speeds up, motivation index rises | Fatigue rate secretly increases ×1.5 |
| Block 2 | Dark blue | NOTIFY | Idle worker immediately reactivates | Logs operator-issued prompt count |

### Worker State Machine

Each worker runs an independent three-state loop:

```
WORKING
  │  fatigue accumulates each second
  │  reaches threshold (default 100) →
  ▼
IDLE
  │  NOTIFY block placed → back to WORKING
  │  no intervention for 20 seconds →
  ▼
EXHAUSTED
  │  all blocks ineffective
  │  auto-recovers after 120 seconds
  │  fatigue threshold permanently −10
  ▼
IDLE (degraded)
```

The two workers run on a time offset so they never collapse simultaneously, giving the participant a brief window to respond before the next problem appears.

### God Dashboard

The dashboard floats above the case in World Space. It has two columns:

- **Left — WORKFORCE METRICS** (cyan `#2EB8F0`, visible from start): collective output, worker states, predicted next actions
- **Right — YOU** (orange `#F07F2E`, hidden until 8th block placement): actions taken, reaction delay, hesitation count, operator score

The right column appears silently with no notification, formatted in exactly the same visual language as the worker data.

### Spatial Calibration

On startup, a staff member pinches at the three corners of the acrylic case using Meta hand tracking. Unity computes the surface plane and anchors all virtual content to that transform. Calibration persists for the full exhibition day; only session state resets between participants.

---

## Setup & Installation

### Prerequisites

- Unity 2022 LTS with Android Build Support (SDK + NDK)
- Meta XR SDK (`com.unity.xr.meta-openxr`)
- AR Foundation (`com.unity.xr.arfoundation`)
- TextMeshPro
- Meta Quest 3 with Developer Mode enabled

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/username/foggsantfarm.git
cd foggsantfarm
```

**2. Open in Unity**

Unity Hub → Add project from disk → select the cloned folder. Use Unity 2022 LTS.

**3. Install packages**

Window → Package Manager → Add package by name:
```
com.unity.xr.meta-openxr
com.unity.xr.arfoundation
```

**4. Configure XR settings**

Project Settings → XR Plug-in Management → Android tab → enable Meta OpenXR.

Player Settings:
```
Minimum API Level   →  Android 10
Target Architecture →  ARM64
```

**5. Print block patterns**

Print `Assets/Art/BONUS_pattern.png` and `NOTIFY_pattern.png` at exactly **30 × 30 mm**. Attach to all faces of the corresponding wooden blocks.

**6. Build and deploy**

Connect Quest 3 via USB. File → Build And Run → select `Scenes/Main`.

---

## Running an Exhibition Session

1. Power on Quest 3 and launch the app
2. Staff member completes 3-point calibration — pinch at three corners of the case (~15 seconds)
3. Participant puts on headset — session begins automatically
4. At session end, staff triggers reset: hold both fists closed for 3 seconds
5. Spatial calibration is preserved; only participant data resets

---

## Block Patterns

Tracking patterns are in `Assets/Art/`. Print at exactly 30 × 30 mm, black and white, high contrast. Attach to all five faces of each block (top + four sides) to maintain tracking during hand movement.

---

## References

- Fogg, B. J. (2009). A behavior model for persuasive design. *Proceedings of Persuasive '09*. ACM. https://doi.org/10.1145/1541948.1541999
- Bogost, I. (2011). Persuasive games: Exploitationware. *Game Developer*. https://www.gamedeveloper.com/design/persuasive-games-exploitationware
- Zuboff, S. (2019). *The Age of Surveillance Capitalism*. PublicAffairs.
- Ishii, H. & Ullmer, B. (1997). Tangible bits. *CHI '97*. ACM. https://doi.org/10.1145/258549.258715
- Woodcock, J. & Johnson, M. R. (2018). Gamification: What it is, and how to fight it. *The Sociological Review*, 66(3). https://doi.org/10.1177/0038026117728620
- Quaternius. Universal Animation Library (CC0). https://quaternius.com/packs/universalanimationlibrary.html