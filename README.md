# Solo 5e DM

A structured AI Game Master for solo **5e‑style fantasy campaigns**, optimized for political intrigue, espionage, mystery, adventure, romance, and tactical combat.

Every campaign is generated collaboratively in Session Zero based on your tastes; it collaborates with you to build an original, custom campaign using 5e mechanics.

If you use this tool/play this game and end up enjoying it, please consider helping to support it.

[![](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/damageplanx)

---

## Supported Platforms

| Platform | Setup | Sharing | Best for |
|---|---|---|---|
| **ChatGPT** (Custom GPT) | Uses `gpt-instructions.md` | Publishable link — anyone can use | Public distribution |
| **Claude** (Project) | Uses `claude-instructions.md` | Per-user only — each player sets up their own | Reliability, prose quality, long sessions |

Both platforms use the same three rules files (`dm-core-rules.md`, `dm-session-zero.md`, `dm-campaign-ops.md`). Only the instructions file differs.

**Which should I use?** If you want to share a link with friends, use ChatGPT. If you want the most reliable rule-following and strongest narrative prose — especially in long sessions — use Claude. Claude puts all files in full context every turn, so it never "forgets" rules that ChatGPT's retrieval system might miss.

---

## Quick Start — ChatGPT

### 1. Create the Custom GPT

1. Go to **ChatGPT → Explore GPTs → Create a GPT**.
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

## Quick Start — Claude

### 1. Create a Claude Project

1. Go to **claude.ai → Projects → Create Project** (requires a Claude Pro subscription).
2. **Name** it something like: `Solo 5e Fantasy DM`.
3. In the **Project Instructions** box, paste the entire content of:

   `docs/claude-instructions.md`

4. In the **Project Knowledge** section, upload all three rules files:

   - `docs/dm-core-rules.md`
   - `docs/dm-session-zero.md`
   - `docs/dm-campaign-ops.md`

5. Start a new conversation within the Project to begin.

> **Note:** Claude Projects aren't shareable as public links. Each player needs to create their own Project and upload the files. The tradeoff is better reliability — Claude loads all files into full context every turn, so the model always has access to every rule simultaneously.

---

## Architecture

The rules are split across multiple files per platform, designed for maintainability and (on ChatGPT) retrieval optimization:

| File | What it contains | When it's needed |
|---|---|---|
| `gpt-instructions.md` / `claude-instructions.md` | Critical rules reinforcement, Session Zero trigger, meta-talk, on-demand commands | Every turn |
| `dm-core-rules.md` | Format, flows, secrets, combat, scene transitions, hazards, NPC rules, conditions, interaction rules, 16 worked examples | Every turn during play |
| `dm-session-zero.md` | Session Zero flow, beginner/experienced mode, character creation, party building, premise summary | Start of campaign; also referenced for mid-campaign companion replacement |
| `dm-campaign-ops.md` | Leveling (including multiclass), subclass timing, loot, travel, rests, world advancement, in-world time, token management, `output for new thread` | Periodically |

**Why the split?**

- On **ChatGPT**, Knowledge files use retrieval (RAG) — the system searches for relevant chunks per turn. Splitting means Session Zero content doesn't pollute combat retrieval, and related rules stay together for coherent chunk pulls.
- On **Claude**, all Project Knowledge is loaded into full context every turn. The split still helps with organization and maintainability — but every rule is visible to the model at all times regardless.
- On **both platforms**, the instructions file carries the 11 rules most likely to conflict with default model behavior (NPC agency, action chains, hidden rolls, etc.) as reinforcement that's always present.

---

## Using the DM

### Session Zero (First Time in a New Chat)

When you open a new conversation:

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
  | `spells` | Prepared/known spell list with slots and concentration status |
  | `recap` | Quick session refresher: key events, decisions, unresolved threads |

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
  3. Open a fresh conversation (same GPT or same Claude Project)
  4. Paste the block and say: *"Continue the campaign from the Last Scene."*

The **`output for new thread`** command makes the DM emit a structured **Campaign State block** — a machine-readable snapshot covering your character (with current HP and resources, not reset to full), companions, world state and in-world time, NPC relationships with current dispositions, active factions, open quest threads, ticking clocks, and the last scene. Verify the values look right before continuing — anything the DM is uncertain about is flagged with a `?`.

> **Tip:** After about 40–50 exchanges, the DM will proactively suggest saving your state and starting a fresh thread. This keeps continuity sharp and prevents context-window degradation.

---

## Files Overview

```
docs/
├── gpt-instructions.md        # ChatGPT Instructions box
├── claude-instructions.md      # Claude Project Instructions
├── dm-core-rules.md            # Format, flows, combat, NPCs, conditions, 16 worked examples
├── dm-session-zero.md          # Session Zero, character creation, party building
└── dm-campaign-ops.md          # Leveling, loot, travel, rests, world advancement, thread export
```

| File | Purpose |
|---|---|
| `gpt-instructions.md` | ChatGPT: paste into the GPT Instructions box. Routes to Knowledge files, reinforces critical rules. |
| `claude-instructions.md` | Claude: paste into Project Instructions. Same critical rules, adapted for full-context architecture. |
| `dm-core-rules.md` | The main rules file — needed every turn during play. Upload as Knowledge (ChatGPT) or Project Knowledge (Claude). |
| `dm-session-zero.md` | Session Zero and character creation. Upload as Knowledge / Project Knowledge. |
| `dm-campaign-ops.md` | Leveling, loot, travel, rests, continuity. Upload as Knowledge / Project Knowledge. |

---

## Worked Examples

`dm-core-rules.md` includes 16 annotated examples that the model uses for style pattern-matching:

| # | Scenario | Demonstrates |
|---|---|---|
| 1 | Social / investigation scene | Flow A auto-roll, Flow C passive reveal, choice menu |
| 2 | Combat with action economy | Combat block, initiative, surprise, action labels |
| 2b | Death saves | Death save prompt, tracker format |
| 3 | Intrigue / romantic tension | Scene establishment, NPC appearance, sensory grounding |
| 4 | Travel with surveillance | Travel time, tailing check, risky route options |
| 4b | Flow C failure | Passive check fails — hidden detail omitted entirely, no hints |
| 5 | NPC death consequences | Faction clocks, quest threads, reputation tracking |
| 6 | NPC agency | Player declares action on NPC, NPC resists, contested check |
| 7 | Action chain | Multiple declared actions resolved one step at a time |
| 8 | Party combat | Mixed player/AI control, separate companion menus |
| 8b | Concentration save | CON save when concentrating character takes damage |
| 9 | Level-up mid-campaign | Milestone announcement, subclass prompt, stat block update (references dm-campaign-ops.md leveling protocol) |
| 10 | Short rest with Hit Dice | Post-combat rest, Hit Dice spending, resource recharge, stat block update |
| 11 | Flow B failure | Prompted roll fails, consequence narration, updated choices reflect failure |
| 12 | NPC social secret (Category 2) | NPC Insight vs PC passive Deception, behavioral shift, no visible mechanic |
| 13 | Opportunity attack and Disengage | PC movement warning, Disengage option, enemy OA prompt |

---

## Customization Tips

- **Change the vibe**: In Session Zero, emphasize different themes (e.g., heists and urban intrigue, planar weirdness, low-combat investigation).
- **Adjust mid-campaign**: Use `(( meta ))` to change Campaign Constants at any time — shift tone, add or remove hard limits, adjust themes.
- **Homebrew**: Describe homebrew races/subclasses during character creation; the DM will incorporate them as long as they're roughly 5e‑compatible.
- **Drift correction**: If you notice the tone, themes, or NPC complexity drifting from what you established in Session Zero, use `(( meta ))` to remind the DM of your Campaign Constants. For example: `(( Remember, the tone should be dark and gritty, not lighthearted. ))` The DM has a built-in drift check, but an explicit reminder is the most reliable way to course-correct.
- **Strict vs soft rules**: The current configuration is **strict** — illegal builds must be fixed before play. If you prefer softer enforcement, tweak that in `dm-session-zero.md`.
- **Ruleset choice**: The DM supports both **2014 (Classic)** and **2024 (Updated)** rules, selected during Session Zero. The model has stronger training on 2014 rules and will flag uncertainty more readily with 2024 mechanics.

---

## Contributing

Issues and pull requests are welcome. If you playtest and find the model consistently fails to follow a rule, that's a high-value bug report — include the scenario and what the model did wrong.

If you add your own house rules or examples, extend the relevant `docs/dm-*.md` file.

---

## License

MIT. Fork, customize, and adapt for your own tables and solo campaigns.
