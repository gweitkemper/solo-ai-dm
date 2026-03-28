# CLAUDE.md — Solo AI DM Project

## Project overview
This is an AI Dungeon Master system for solo D&D 5e campaigns. It consists of
modular Markdown instruction files loaded into Claude Projects or ChatGPT Custom GPTs.
There is no application code — only prompt/instruction documents and documentation.

## Repository structure
- `docs/` — all game files (rules, instructions, platform configs)
  - `dm-core-rules.md` — runtime DM behavior (~1,200 lines)
  - `dm-session-zero.md` — character creation and campaign setup (~470 lines)
  - `dm-campaign-ops.md` — campaign state, rests, travel, level-up (~395 lines)
  - `claude-instructions.md` — Claude Project Instructions field content
  - `gpt-instructions.md` — ChatGPT Custom GPT Instructions box content
- `ops/` — improvement workflow prompts and audit outputs
- `README.md` — public-facing setup guide
- `CHANGELOG.md` — version history

## Editing rules
- **Surgical edits only.** When fixing a specific finding, change only what's specified.
  Do not restructure, reorder, or rewrite sections outside scope.
- **Preserve formatting exactly.** These files are loaded into AI platforms as system
  prompts. Whitespace, indentation, and section headers are load-bearing.
- **No line wrapping changes.** Do not reflow paragraphs. Diffs should show only
  substantive changes.
- **Cross-file consistency.** If a stat block format, on-demand command, or rules term
  is changed in one file, check all other files for matching references.
- **Claude + GPT parity.** The two instruction files must stay in sync on all gameplay
  rules. They differ only in how they reference the rules files (full context vs RAG).

## Commit conventions
- `fix:` — corrects a rules error, mechanical bug, or cross-file inconsistency
- `feat:` — adds new game functionality (new command, new rules section, etc.)
- `docs:` — README, CHANGELOG, ops/ files
- `refactor:` — reorganizes without changing behavior

## What NOT to do
- Do not add application code, build tools, or package.json
- Do not change the file structure without explicit instruction
- Do not auto-format or lint the markdown files
- Do not push directly to main — always use a branch + PR
