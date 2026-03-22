# Solo D&D 5e AI Dungeon Master

A structured AI Dungeon Master for solo D&D 5e campaigns, optimized for political intrigue, espionage, mystery, romance, and tactical combat.  
Designed to run as a **ChatGPT Custom GPT** powered by:

- A short core instruction file: `docs/gpt-instructions.md`
- A detailed rules & workflow spec: `docs/dm-rules.md`
- Example transcripts: `docs/examples.md`

---

## Quick Start

### 1. Create the Custom GPT

1. Go to **ChatGPT → GPTs → Create a GPT**.
2. **Name** it something like: `Solo D&D 5e DM`.
3. In the **Instructions** box, paste the entire content of:

   `docs/gpt-instructions.md`

4. In the **Knowledge** / files section, upload:

   - `docs/dm-rules.md`
   - `docs/examples.md` (optional but recommended as extra style guidance)

5. Capabilities (recommended):

   - Turn **off** Code Interpreter and image generation.
   - Leave **Web Search** enabled (the rules tell the DM to use it mainly during Session Zero, or when you explicitly ask via `((meta))`).

6. Add a few **conversation starters**, for example:

   ```text
   - Start Session Zero for a new solo D&D 5e campaign.
   - Continue my ongoing solo campaign from where we left off.
   - Help me build a new character step by step.

7. Save the GPT.

---

## Using the DM

### Session Zero (First Time in a New Chat)

When you open the GPT for the first time in a new conversation:

1. The DM will **run Session Zero** (mandatory):
   - Ask about themes (political intrigue, espionage, horror, romance, etc.), tone, and content boundaries.
   - Optionally ask for **media inspirations** (books/shows/movies/games) and may use web search to pull high-level themes.
   - Ask whether you want a **solo character or a party**, and how many party members (1–4).

2. **Character creation / import**:
   - You can:
     - Import an existing character (e.g., by providing screenshots of a sheet), or
     - Build a new one step by step using 5e rules.
   - The DM:
     - Parses screenshot details, summarizes them, and asks you to confirm/correct.
     - Walks you through name, race, class, background, stats, skills, spells, feats, etc.
     - Enforces **strict 5e legality** (no starting until rule issues are resolved).

3. **Party building** (if you chose more than one member):
   - For each extra member:
     - Choose import, step-by-step creation, or auto-generated companion.
     - Decide if they are **player-controlled or AI-controlled in combat**.
   - The DM designs companions to create a **well-rounded adventuring party** (tank, support, utility, damage/control).

4. The DM outputs a concise **Party & Premise Summary**, then starts **Chapter One**.

---

## Playing Sessions

- The DM:
  - Presents scenes with a **header + character block**, a **short rules reminder**, then narrative.
  - Calls for rolls (e.g. “Roll Perception; tell me when you’re ready and I’ll roll”), waits for your confirmation, and then rolls virtually and shows the math.
  - Ends major beats with a **choice menu**: A/B/C plus “D) Something else entirely — just tell me.”
  - Appends a **📜 Session Log** each turn (location, time, party, NPCs, clues, quests).

- You:
  - Reply in natural language or by choosing A/B/C/D.
  - Use the **meta channel** for out-of-character talk:

    (( Ask rules questions, tweak tone, enable web search, change who controls companions, etc. ))

  - Between sessions, copy the 📜 Session Log into your own notes.

---

## Multi‑Session Campaign Flow

A simple loop for long campaigns:

Session 1  
- Run Session Zero  
- Start Chapter One  
- Copy 📜 Session Logs into your notes  

Session 2+  
- Either continue in the same chat, or:  
  - Type: `output for new thread`  
  - Copy the generated starter block  
  - Open a fresh chat with the same GPT  
  - Paste the starter block and say “Continue from the Current Situation & Hooks.”

The **`output for new thread`** command makes the DM emit a reusable campaign starter (summary, current character block, key NPCs/factions, and current hooks) so you can safely move to a new chat when context gets long.

---

## Files Overview

| File                     | Purpose                                                                 |
|--------------------------|-------------------------------------------------------------------------|
| `README.md`              | This guide                                                             |
| `docs/gpt-instructions.md` | Short “core” instructions to paste into the GPT builder               |
| `docs/dm-rules.md`       | Full rules, Session Zero flow, party/character logic, and formats      |
| `docs/examples.md`       | Example exploration, combat, and social scenes for style reference     |

If you add your own house rules or examples, extend `dm-rules.md` and/or `examples.md` and bump your repo changelog.

---

## Customization Tips

- **Change the vibe**: In Session Zero, emphasize different themes (e.g., heists and urban intrigue, planar weirdness, low-combat investigation).
- **Homebrew**: Describe homebrew races/subclasses during character creation; the DM will incorporate them as long as they’re roughly 5e‑compatible.
- **Strict vs soft rules**: The current configuration is **strict**—illegal builds must be fixed before play. If you prefer softer enforcement, tweak that in `dm-rules.md`.

---

## License

MIT. Fork, customize, and adapt for your own tables and solo campaigns.
