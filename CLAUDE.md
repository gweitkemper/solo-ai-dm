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

## File roles and platform context
The system runs on two platforms simultaneously:
- **Claude Projects** — all 3 rules files loaded as full context
- **ChatGPT Custom GPT** — rules files uploaded to Knowledge (RAG retrieval)

Cross-file dependency gaps only affect ChatGPT. When flagging these, label them
"ChatGPT-only" so they aren't confused with Claude-side bugs.

The two instruction files (claude-instructions.md, gpt-instructions.md) must stay
in sync on all gameplay rules. They differ ONLY in how they reference the rules files.

## Editing rules
- **Surgical edits only.** When fixing a specific finding, change only what's specified.
  Do not restructure, reorder, or rewrite sections outside scope.
- **Preserve formatting exactly.** These files are loaded into AI platforms as system
  prompts. Whitespace, indentation, and section headers are load-bearing.
- **No line wrapping changes.** Do not reflow paragraphs. Diffs should show only
  substantive changes.
- **Cross-file consistency.** If a stat block format, on-demand command, or rules term
  is changed in one file, check all other files for matching references.
- **Instruction file parity.** After editing any gameplay rule in one instruction file,
  apply the same edit to the other instruction file.

## When implementing fixes
1. Create a branch named `fix/<short-description>` or `feat/<short-description>`.
2. Make the minimum changes needed. Do not touch unrelated sections.
3. After editing, verify cross-file consistency: search all docs/ files for any term
   or format you changed to ensure nothing is now inconsistent.
4. Commit with a conventional message: `fix: <description>` or `feat: <description>`.
5. Open a PR with a clear description of what was changed and why.
6. Never push directly to main.

## When implementing feature proposals
1. Only implement features that have been explicitly approved in an issue comment.
2. Write the new content in the style and tone of the surrounding file content.
3. If the feature requires changes to multiple files, make all changes in the same PR.
4. In the PR description, list every file changed and what was added/modified.

## D&D rules reference
The system targets D&D 5e 2014 PHB rules by default. When verifying mechanical accuracy:
- Proficiency bonus by level: +2 (1-4), +3 (5-8), +4 (9-12), +5 (13-16), +6 (17-20)
- Ability modifier = (score - 10) / 2, rounded down
- Saving throw proficiency is class-dependent (2 saves per class)
- Passive checks = 10 + modifier + proficiency (if proficient)

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
- Do not implement features unless explicitly approved in an issue comment
