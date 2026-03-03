# Failure Archive: LoL Death Log  
A personal micro-dataset that turns “failure” into a readable narrative and an audible event (Small Data)

## Overview
I treat failure in games as personal, small-scale data. Every time I die or fail in *League of Legends* (LoL), I log a few fields: timestamp, match/round index, in-game minute, map area, game phase, cause of failure, whether I had vision, and a self-rated emotional intensity (1–5).

This work is **not** about using data to “improve performance.” Instead, it reframes failure as a legible narrative system:
- When does failure happen?
- Which mechanics repeat?
- How does emotion accumulate or fade over time?
- How do the categories I define shape what can be seen and understood?

The outcome is a set of **visual and sonic outputs**: a failure timeline / heat-like field, distributions of causes and areas, and a simple sonification process where each failure becomes a sound event. The audience is invited into the process—how categories are defined and how the archive is constructed—so the **data system itself becomes part of the artwork**.

---

## Dataset (CSV)
File: `data/deaths.csv`  
Columns:

- `date` — log date (YYYY-MM-DD)  
- `match` — match identifier (per game session)  
- `minute` — in-game minute of the failure (can be decimal)  
- `area` — map area (Top / Jungle / Mid / Bot / River / Dragon / Baron / Base)  
- `phase` — game phase (Laning / Mid / Late)  
- `cause` — failure cause (Gank / Vision / Objective / Teamfight / Overextend / Solo / Towerdive / Misplay)  
- `vision` — vision state (Yes / No / Unknown)  
- `emotion` — self-rated intensity (1–5)

> Note: This is a small personal dataset. It does not include player IDs or chat logs.

---

## Visualization & Mapping Logic
The main output is a **single-canvas dashboard**: a large “diffuse dye / blot” field (match × time) plus embedded summaries (Area/Cause mini bars and minute density) rendered in the same gradient/ink-bleed visual style.

### Main Field (match × time)
Each row in the dataset is drawn as a **diffuse blot** rather than a traditional point or bar, emphasizing residue, repetition, and emotional atmosphere.

Mapping:
- **x-axis → `minute`** (when the failure happened)
- **y-axis → `match`** (each match occupies a row)
- **size → `emotion`** (higher intensity = larger/stronger blot)
- **base color palette → `emotion` (5 gradient groups)**  
  Emotion 1–5 maps to five two-color gradients (“dye pairs”), producing a chromatography-like visual texture.
- **secondary dye layer → `cause`**  
  Each cause adds an additional tint layer, making blots richer and more varied even within the same emotion group.
- **smear / drag strength → `phase`**  
  Laning → Mid → Late increases diffusion and “residue.”
- **smear direction → `area`**  
  Different map areas steer the diffusion direction, creating a subtle flow-field.
- **neon ring → `vision = Yes`**  
  If vision is “Yes,” the blot gains a brighter halo, suggesting “being seen / being tracked.”

### Sidebar Summaries (embedded)
On the right side of the same canvas:
- **Area distribution** (Top/Mid/Bot…)
- **Cause distribution** (Gank/Objective…)

These mini charts use the same gradient/ink-bleed styling to maintain a unified visual language.

### Bottom Strip
- **Minute density**: a 0–40 minute histogram showing where failures cluster over time.

---

## Sonification (Hover-to-Sound)
The project includes a **hover-to-sound** interaction: when the viewer moves the cursor near a blot, the corresponding failure event triggers audio.

### Audio Mapping
- **pitch ← `minute`**: later minutes map to higher pitch (a sense of progression/tension)
- **loudness/energy ← `emotion`**: higher emotion = louder and more intense
- **timbre (waveform) ← `cause`**: different causes use different waveforms (sine/triangle/square/sawtooth)
- **stereo position (pan) ← `area`**: different areas map across the stereo field
- **envelope release ← `phase`**: Late has a longer tail (failure residue)
- **extra ping ← `vision = Yes`**: a short high-frequency ping layered on top

> Implementation uses the Web Audio API. Most browsers require an initial user gesture to unlock audio, so click the canvas once before hovering if sound does not start immediately.

---

## How to Run
Because the project loads `data/deaths.csv` via `fetch()`, you must run a local HTTP server (do not open the page via `file://`).

### Option A: VS Code Live Server
1. Open the project folder in VS Code  
2. Start Live Server  
3. Visit the local URL it provides

### Option B: Python HTTP Server
Run in the project root:
```bash
python3 -m http.server 8000
