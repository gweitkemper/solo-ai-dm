# Solo 5e DM

A structured AI Game Master for solo **5e‑style fantasy campaigns**, optimized for political intrigue, espionage, mystery, adventure, romance, and tactical combat.

Every campaign is generated collaboratively in Session Zero based on your tastes; it collaborates with you to build an original, custom campaign using 5e mechanics.

If you use this tool/play this game and end up enjoying it, please consider helping to support it.

[![](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/damageplanx)

Designed to run as a **Custom GPT** (ChatGPT) or similar LLM platform, powered by:

- A core instruction file (always in context): `docs/gpt-instructions.md`
- Three Knowledge base files (retrieved as needed):
  - `docs/dm-core-rules.md` — format, flows, combat, NPC rules, worked examples
  - `docs/dm-session-zero.md` — Session Zero, character creation, party building
  - `docs/dm-campaign-ops.md` — leveling, loot, travel, rests, campaign continuity

---

## Quick Start

### 1. Create the Custom GPT

1. Go to **ChatGPT → Explore GPTs → Create a GPT** (or your preferred LLM platform).
2. **Name** it something like: `Solo 5e Fantasy DM`.
3. In the **Instructions** box, paste the entire content of:

   `docs/gpt-instructions.md`

4. In the **Knowledge** / files section, upload all three rules files:

   - `docs/dm-core-rules.md`
   - `docs/dm-session-zero.md`
   - `docs/dm-campaign-ops.md`

5. Capabilities (recommended):

   - Turn **off** Code Interpreter and image generation.
   - Leave **Web Search** enabled (the rules tell the DM to use it mainly during Session Zero, or when you explicitly ask via `((meta))`).

6. Add a few **conversation starters**, for example:

   ```
   - Start Session Zero for a new solo 5e-style fantasy campaign.
   - Continue my ongoing solo campaign from where we left off.
   - Help me build a new character step by step.
   ```

7. Save the GPT.

---

## Architecture

The rules are split across four files, designed around how Custom GPTs actually retrieve information:

| File | Lives in | Retrieved when | Lines |
|---|---|---|---|
| `gpt-instructions.md` | Instructions box | **Every turn** (always in context) | ~120 |
| `dm-core-rules.md` | Knowledge base | **Every turn** during active play | ~1,170 |
| `dm-session-zero.md` | Knowledge base | **Start of campaign** only | ~420 |
| `dm-campaign-ops.md` | Knowledge base | **Periodically** (leveling, rests, loot, thread export) | ~310 |

**Why the split matters:** The Instructions box is always in context — the model sees it every turn. Knowledge files use retrieval — the system searches for relevant chunks based on the conversation. Splitting the rules means:

- **Session Zero content** (character creation, party building) doesn't pollute retrieval during play, when the model needs combat or NPC rules instead.
- **The 10 most critical rules** live in `gpt-instructions.md` so they're never subject to retrieval luck — they're always present. These include player agency, NPC agency on player-declared actions, action chain processing, hidden NPC rolls, and drift correction.
- **Related rules stay together** in the same file, so retrieval is more likely to pull a complete, coherent rule set rather than fragments from different sections.

---

## Using the DM

### Session Zero (First Time in a New Chat)

When you open the GPT for the first time in a new conversation:

1. The DM will **run Session Zero** (mandatory):
   - Ask about themes (political intrigue, espionage, horror, romance, etc.), tone, and content boundaries.
   - Ask which **ruleset** to use — Classic (2014) or Updated (2024) — framed in plain language, no version numbers.
   - Optionally ask for **media inspirations** (books/shows/movies/games) and may use web search to pull high-level themes.
   - Ask whether you want a **solo character or a party**, and how many party members (1–4).

2. **Character creation / import**:
   - You can:
     - Import an existing character (e.g., by providing screenshots of a sheet), or
     - Build a new one step by step using 5e rules.
   - The DM:
     - Detects whether you're a **beginner or experienced** player and adjusts explanation depth accordingly.
     - Walks you through name, race, class, background, stats, skills, spells, feats, etc.
     - Enforces **strict 5e legality** (no starting until rule issues are resolved).
     - Shows a **running character summary** before finalizing, so you can catch errors before earlier messages scroll away.

3. **Party building** (if you chose more than one member):
   - For each extra member:
     - Choose import, step-by-step creation, or auto-generated companion.
     - Decide if they are **player-controlled or AI-controlled in combat**.
   - The DM designs companions to create a **well-rounded adventuring party** (tank, support, utility, damage/control).

4. The DM outputs a concise **Party & Premise Summary**, locks **Campaign Constants** (themes, tone, limits, setting, inspirations, ruleset), then starts **Chapter One**.

---

### Playing Sessions

- The DM:
  - Presents scene narrative under a chapter heading, written in second person ("you").
  - Handles rolls using three flows:
    - **Flow A** — auto-rolls before presenting choices when the result shapes what you can perceive.
    - **Flow B** — prompts for a roll after you choose an action that requires one.
    - **Flow C** — resolves passive checks silently using your static passive value (no d20), revealing only what your character notices on success.
  - Ends major beats with a **choice menu**: A/B/C plus "D) Something else entirely — just tell me."
  - Shows a compact **character stat block** at the end of each response (HP, AC, resources, equipment, stats). Conditions appear only when one is active.
  - **Never auto-executes your declared outcomes on NPCs** — NPCs have agency and may resist, refuse, or react differently based on who they are.
  - **Processes action chains one step at a time** — if you declare five actions in one message, the DM resolves only up to the first check or NPC reaction, then presents the new situation.

- You:
  - Reply in natural language or by choosing A/B/C/D.
  - Use the **meta channel** for out-of-character talk:

    `(( Ask rules questions, tweak tone, adjust campaign constants, enable web search, change who controls companions, flag errors, etc. ))`

- **On-demand commands** — type these keywords at any time:

  | Keyword | What you get |
  |---|---|
  | `rules` | Short rules reminder for the current situation |
  | `log` | Session Log — copy this into your notes between sessions |
  | `inventory` | Full categorized inventory with coin |
  | `npcs` | NPC relationship tracker with current dispositions |
  | `factions` | Faction tracker with goals and attitudes toward you |
  | `stats` | Proficient skills, tool proficiencies, passive values |

---

## Multi‑Session Campaign Flow

A simple loop for long campaigns:

**Session 1**
- Run Session Zero
- Start Chapter One
- Copy `log` output into your notes

**Session 2+**
- Either continue in the same chat, or:
  1. Type: `output for new thread`
  2. Copy the generated Campaign State block
  3. Open a fresh chat with the same GPT
  4. Paste the block and say: *"Continue the campaign from the Last Scene."*

The **`output for new thread`** command makes the DM emit a structured **Campaign State block** — a machine-readable snapshot covering your character (with current HP and resources, not reset to full), companions, world state and in-world time, NPC relationships with current dispositions, active factions, open quest threads, ticking clocks, and the last scene. Verify the values look right before continuing — anything the DM is uncertain about is flagged with a `?`.

> **Tip:** After about 40–50 exchanges, the DM will proactively suggest saving your state and starting a fresh thread. This keeps continuity sharp and prevents context-window degradation.

---

## Files Overview

| File | Purpose |
|---|---|
| `README.md` | This guide |
| `docs/gpt-instructions.md` | Core instructions for the GPT Instructions box — routing + 10 critical always-in-context rules |
| `docs/dm-core-rules.md` | Format, roll flows, secrets handling, player declaration processing, combat, scene transitions, environmental hazards, NPC rules, conditions, interaction rules, and 9 worked examples |
| `docs/dm-session-zero.md` | Session Zero flow, beginner/experienced mode, character creation, party building, premise summary |
| `docs/dm-campaign-ops.md` | Leveling (including multiclass), subclass timing, loot, travel, rests, world advancement, in-world time, token management, `output for new thread` command |

---

## Customization Tips

- **Change the vibe**: In Session Zero, emphasize different themes (e.g., heists and urban intrigue, planar weirdness, low-combat investigation).
- **Adjust mid-campaign**: Use `(( meta ))` to change Campaign Constants at any time — shift tone, add or remove hard limits, adjust themes.
- **Homebrew**: Describe homebrew races/subclasses during character creation; the DM will incorporate them as long as they're roughly 5e‑compatible.
- **Strict vs soft rules**: The current configuration is **strict** — illegal builds must be fixed before play. If you prefer softer enforcement, tweak that in `dm-session-zero.md`.
- **Ruleset choice**: The DM supports both **2014 (Classic)** and **2024 (Updated)** rules, selected during Session Zero. The model has stronger training on 2014 rules and will flag uncertainty more readily with 2024 mechanics.

---

## Worked Examples

`dm-core-rules.md` includes 9 annotated examples that the model uses for style pattern-matching:

| # | Scenario | Demonstrates |
|---|---|---|
| 1 | Social / investigation scene | Flow A auto-roll, Flow C passive reveal, choice menu |
| 2 | Combat with action economy | Combat block, initiative, surprise, action labels |
| 2b | Death saves | Death save prompt, tracker format |
| 3 | Intrigue / romantic tension | Scene establishment, NPC appearance, sensory grounding |
| 4 | Travel with surveillance | Travel time, tailing check, risky route options |
| 5 | NPC death consequences | Faction clocks, quest threads, reputation tracking |
| 6 | NPC agency | Player declares action on NPC, NPC resists, contested check |
| 7 | Action chain | Multiple declared actions resolved one step at a time |
| 8 | Party combat | Mixed player/AI control, separate companion menus |
| 9 | Level-up mid-campaign | Milestone announcement, subclass prompt, stat block update |

---

## Contributing

Issues and pull requests are welcome. If you playtest and find the model consistently fails to follow a rule, that's a high-value bug report — include the scenario and what the model did wrong.

If you add your own house rules or examples, extend the relevant `docs/dm-*.md` file.

---

## License

MIT. Fork, customize, and adapt for your own tables and solo campaigns.
