# Solo AI DM — Continuous Improvement Operations

A prompt-driven system for reviewing, improving, and maintaining the solo-ai-dm project.
Designed for use with Claude (Opus) conversations, Claude Code for git operations, and GitHub Issues for tracking.

---

## How This System Works

```
┌─────────────────────────────────────────────────────────────────┐
│                    IMPROVEMENT CYCLE                            │
│                                                                 │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐  │
│  │  AUDIT   │───►│ PROPOSE  │───►│ APPROVE  │───►│ PUBLISH  │  │
│  │ Prompt 1 │    │ Prompt 2 │    │ (You)    │    │ Prompt 4 │  │
│  └──────────┘    └──────────┘    └──────────┘    └──────────┘  │
│       │               │                               │         │
│       ▼               ▼                               ▼         │
│  Findings doc    GitHub Issues                   Repo updated   │
│  (markdown)      (feature + fix)                 (via Claude    │
│                                                   Code or PR)   │
│                                                                 │
│  ┌──────────┐                                                   │
│  │ SESSION  │  (Run after playtesting to feed findings back in) │
│  │ Prompt 3 │                                                   │
│  └──────────┘                                                   │
└─────────────────────────────────────────────────────────────────┘
```

There are four core prompts, each designed for a fresh Claude conversation to avoid context drift. Each prompt is self-contained — you paste it, attach the files it specifies, and get a structured output you can act on.

**Why fresh conversations?** Your project files total ~2,500 lines across 6 files. Auditing them thoroughly uses most of a context window. A reviewer that also carries 50k tokens of prior design discussion will be less critical and more likely to confirm what it "remembers" rather than catch real issues. Fresh eyes, every time.

---

## Before You Start: Repo Sync

Your GitHub repo (`gweitkemper/solo-ai-dm`) is confirmed current as of 2026-03-28 — all 6 files
in `docs/` reflect the latest post-audit build (character sheet audit, cross-file dependency fixes,
cleanup pass all applied).

To complete the ops setup, add these new files to the repo:

```
solo-ai-dm/
├── README.md                ← current
├── CHANGELOG.md             ← new, tracks versions
├── CLAUDE.md                ← new, for Claude Code (see Prompt 4)
├── docs/
│   ├── dm-core-rules.md     ← current (post-audit)
│   ├── dm-session-zero.md   ← current (post-audit)
│   ├── dm-campaign-ops.md   ← current (post-audit)
│   ├── claude-instructions.md ← current
│   └── gpt-instructions.md  ← current
└── ops/
    ├── solo-ai-dm-ops.md    ← this file (new)
    └── findings/            ← audit outputs go here (new)
```

---

## Prompt 1 — AUDIT

**Purpose:** Comprehensive review of all project files. Produces a structured findings document.

**When to run:** Every 2–4 weeks, after any major edit pass, or after a playtest reveals issues.

**Attach:** All 6 project files (dm-core-rules.md, dm-session-zero.md, dm-campaign-ops.md, claude-instructions.md, gpt-instructions.md, README.md). Paste them inline if you want to preserve tool-call budget for web search.

**Context management note:** This prompt asks the model to read ~2,500 lines of rules files. If you hit output limits, split into two passes: Pass A covers the 3 rules files, Pass B covers the 2 instruction files + README + cross-file checks. The prompt is annotated with `[SPLIT POINT]` to show where to divide.

---

### Prompt 1 text:

```markdown
You are an expert in tabletop RPG design, AI system prompt engineering, and solo TTRPG play systems. I'm going to give you the complete file set for my AI Dungeon Master project — a system that runs solo D&D 5e campaigns on Claude Projects and ChatGPT Custom GPTs.

Read every file completely before responding. Do not start analyzing until you've finished reading.

## Files in this project

| File | Role |
|------|------|
| dm-core-rules.md | Runtime DM behavior: combat, social, exploration, stat blocks, on-demand commands, worked examples |
| dm-session-zero.md | Character creation flow, party building, campaign setup |
| dm-campaign-ops.md | Campaign state export, level-up, rests, travel, token management, session logs |
| claude-instructions.md | Platform-specific instructions loaded into Claude Project Instructions field |
| gpt-instructions.md | Platform-specific instructions loaded into Custom GPT Instructions box |
| README.md | Public-facing documentation and setup guide |

The system is designed for two platforms:
- **Claude Projects** — all 3 rules files loaded as Project Knowledge (full context)
- **ChatGPT Custom GPT** — rules files uploaded to Knowledge (RAG retrieval, not guaranteed full context)

## Audit dimensions

Analyze every file against these dimensions. For each finding, include: **Location** (file, section, approximate line or quote), **Issue** (what's wrong), **Severity** (Critical / High / Medium / Low), and **Recommendation** (specific fix).

### 1. Internal contradictions
Rules that conflict with each other — following one requires violating another. Quote both passages.

### 2. Coverage gaps
Situations likely to arise in play that the rules don't address. Focus on gaps that cause inconsistent improvisation rather than acceptable DM judgment calls. Consider: unusual player actions, edge cases in combat/social/exploration, transitions between scene types, multiclass interactions, multi-session continuity.

### 3. D&D 5e rules accuracy
Verify mechanical claims against 5e RAW (2014 PHB). Check: stat block math in all worked examples (modifiers, proficiency, damage dice, spell slots), multiclass prerequisites, saving throw proficiency assignments by class, spell preparation rules, level-up gains, rest recharge rules. If you're uncertain about a specific rule, say so — don't guess.

### 4. LLM attention risks
Rules that are well-written but likely to be deprioritized by the model under context pressure. Look for: rules buried deep in long sections, rules stated once with no reinforcement, rules that fight the model's default behavior, instructions the model would need to override its training to follow. Suggest how to make them more resilient.

### 5. Cross-file dependency gaps (ChatGPT RAG)
Situations where ChatGPT retrieves File A but needs information from File B to execute correctly. For each finding, state the trigger, which file gets retrieved, what's missing, and the gameplay impact. Claude (full context) is unaffected by these — label them as ChatGPT-only.

[SPLIT POINT — if splitting, end Pass A here. Pass B starts at dimension 6.]

### 6. Platform parity
Compare claude-instructions.md and gpt-instructions.md line by line. The 10 critical rules, on-demand commands, and Session Zero requirements should be identical. Flag divergences not explained by the platform difference.

### 7. README accuracy
Does the README accurately describe the current file structure, setup process, and gameplay flow? Flag any references to old file names, removed features, or outdated architecture.

### 8. Token efficiency
Sections that are unnecessarily verbose and could be tightened. Flag: repeated information within the same file, excessive caveats that don't change behavior, sections that could be condensed without losing clarity. Every token costs model attention.

### 9. Missing or outdated worked examples
Do all worked examples follow the rules described in the same file? Does every complex behavior have at least one example? Flag examples that contradict rules and behaviors with no example.

### 10. Tone and audience fit
Is language consistently pitched at the target audience (new-to-D&D players, video game RPG players)? Flag jargon used before it's defined, sections that assume tabletop experience, and beginner mode content that isn't genuinely accessible.

## Output format

For each dimension, list findings as numbered items. If a dimension has no findings, say "No findings" — do not invent issues.

At the end, produce:

**Summary table:**
| # | Dimension | File | Severity | One-line description |
|---|-----------|------|----------|---------------------|

**Top 5 priorities** — the findings with the highest (severity × likelihood of occurring in play) that I should fix first.

**Feature opportunities** — up to 5 things the system doesn't do yet that would meaningfully improve the play experience, based on gaps you noticed. These are not bugs — they're new capabilities. Describe each in 2–3 sentences: what it is, why it matters, and roughly where it would live in the file structure.
```

---

## Prompt 2 — PROPOSE

**Purpose:** Takes audit findings and converts them into actionable work items: specific edits for fixes, and scoped proposals for new features.

**When to run:** After you've reviewed the audit output from Prompt 1 and decided which findings to act on.

**Attach:** The audit findings document from Prompt 1, plus all project files that will be modified.

---

### Prompt 2 text:

```markdown
You are a technical editor and system architect for an AI Dungeon Master project (solo D&D 5e on Claude Projects and ChatGPT Custom GPTs). I'm attaching the audit findings from my most recent review, plus the project files that need changes.

Your job is to convert approved findings into two types of deliverables:

## A. Fix specifications (for approved fixes)

For each finding I've marked as APPROVED below, produce a **fix spec** that includes:

1. **File:** which file to edit
2. **Location:** section name and enough surrounding context to find the edit point
3. **Current text:** what's there now (quote it)
4. **New text:** what it should say (write the replacement)
5. **Rationale:** one sentence on why this fixes the issue

Keep fixes **surgical** — minimum changes that close the gap. Do not restructure, reorder, or rewrite anything outside the specified scope. If a fix touches multiple files, list each file's changes separately.

## B. Feature proposals (for approved feature ideas)

For each feature I've marked as APPROVED below, produce a **feature proposal** that includes:

1. **Feature name:** short descriptive name
2. **Problem it solves:** 1–2 sentences
3. **Where it lives:** which file(s) and section(s)
4. **Specification:** detailed description of the new behavior, written as if it were already in the rules file. Include any new instructions, formats, or examples needed.
5. **Dependencies:** does this require changes to other files for consistency?
6. **Estimated scope:** number of lines added/changed, rough complexity (small/medium/large)
7. **Risks:** what could go wrong or what might this conflict with?

Do NOT implement features — only specify them. I will review proposals and approve implementation separately.

## Approved findings

[PASTE YOUR SELECTIONS HERE — e.g.:]
[APPROVED FIX: Finding #3 — stat block math error in Example 7]
[APPROVED FIX: Finding #8 — rest recharge table missing Arcane Recovery slot limit]
[APPROVED FEATURE: Feature opportunity #2 — encounter difficulty scaling]
[DEFER: Finding #5 — low severity, will revisit next cycle]

## Rules for this pass

- Do not modify any file content directly. Output specifications only.
- If a fix requires a judgment call (two reasonable implementations), present both options and explain the tradeoff. I'll pick.
- If a feature proposal reveals that the audit missed a prerequisite, flag it.
- Output fix specs first, then feature proposals, in the order I listed them.
```

---

## Prompt 3 — SESSION REVIEW

**Purpose:** Analyzes playtest transcripts against the rules files to find where the DM deviated, what worked well, and what the rules failed to address.

**When to run:** After every 1–2 play sessions.

**Attach:** The session transcript(s) as .docx or pasted text, plus all 3 rules files.

**Context management note:** Session transcripts can be very long (3,000–4,000+ lines). If attaching two transcripts plus three rules files, you will likely hit context limits. Best practice: do one transcript per conversation, or split long transcripts at natural session breaks. Paste the rules files inline to save tool-call budget for reading the transcript.

---

### Prompt 3 text:

```markdown
You are a quality assurance reviewer for an AI Dungeon Master system that runs solo D&D 5e campaigns. I'm attaching a session transcript and the three rules files that define how the DM should behave.

Read the rules files first to understand the expected behavior, then read the transcript and analyze it against those expectations.

## Analysis dimensions

### 1. Mechanical accuracy
Check every roll, modifier, damage calculation, and resource expenditure in the transcript against 5e RAW (2014 PHB) and the stat block shown in the transcript. Flag:
- Wrong modifiers (ability mod, proficiency, or both)
- Incorrect damage dice or damage types
- Class features used incorrectly or not offered when they should have been
- Spell slot/resource tracking errors
- Rest recharge errors
- Saving throw proficiency errors

For each error, show: what the transcript says, what it should say, and the correct math.

### 2. Rules compliance
Where did the DM follow the rules files correctly? Where did it deviate? Focus on:
- Stat block format — does it match the format spec in dm-core-rules.md?
- On-demand commands — when the player used one, was the output format correct?
- Session Log — present and correctly formatted?
- Choice menus — A/B/C/D format followed?
- Combat flow — initiative, turns, conditions tracked?
- Meta-talk channel — respected and not leaked into narrative?

### 3. Gaps revealed by play
Situations that came up in the transcript that the rules don't address or handle ambiguously. These are the most valuable findings — they represent real gameplay moments where the system needed guidance and didn't have it.

### 4. Narrative and engagement quality
Not a rules check — a play experience assessment:
- Were NPC voices distinct and consistent?
- Did choices feel meaningful (real consequences, not just flavor)?
- Was pacing good (not too much exposition, not too rushed)?
- Did the DM balance player agency with story momentum?

### 5. Character data integrity
Compare the character's stats at the end of the transcript against what they should be based on the character creation and any level-ups during the session. Flag any drift.

## Output format

**Mechanical Errors** — numbered list, each with location in transcript, what's wrong, correct value
**Rules Compliance** — pass/fail checklist for each rules area, with notes on deviations
**Gaps Revealed** — numbered list, each with the situation, what the DM did, what guidance was missing
**Narrative Assessment** — 1 paragraph, honest
**Character Integrity** — stat block comparison (transcript vs. expected)

**Summary:** Overall quality score (1–10), top 3 things working well, top 3 things to fix.
```

---

## Prompt 4 — PUBLISH (Repo Sync + Claude Code Setup)

**Purpose:** Get your repo up to date and set up Claude Code for ongoing git operations.

**When to run:** After any approved changes are implemented, or whenever the repo falls behind your working files.

This is the only prompt you don't paste into a Claude conversation — it's a setup guide you follow yourself, plus a CLAUDE.md file for your repo.

---

### Step 1: Add ops files to repo

Your `docs/` folder is already current. Just add the new operations files:

```bash
cd solo-ai-dm

# Create ops directory
mkdir -p ops/findings

# Copy in the new files
cp <path-to>/solo-ai-dm-ops.md ops/
cp <path-to>/CLAUDE.md .
cp <path-to>/CHANGELOG.md .

# Stage and commit
git add -A
git commit -m "docs: add continuous improvement ops workflow

- Add CLAUDE.md for Claude Code integration
- Add CHANGELOG.md to track version history
- Add ops/ directory with audit prompts and workflow"

git push origin main
```

### Step 2: Add CLAUDE.md to repo

Create this file at the repo root. Claude Code reads it automatically:

```markdown
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
```

### Step 3: Set up Claude Code (recommended workflow going forward)

Install Claude Code if you haven't:
```bash
npm install -g @anthropic-ai/claude-code
```

Then, from your repo directory, you can use natural language to manage changes:

```bash
# After reviewing and approving fix specs from Prompt 2:
claude "Apply this fix to docs/dm-core-rules.md: [paste the fix spec]"

# Review what changed:
claude "Show me the diff of what you just changed"

# Commit and push:
claude "Create a branch called fix/rest-recharge-table, commit these changes, and open a PR"
```

For larger feature implementations:
```bash
claude "Read the feature proposal in ops/findings/latest-audit.md, section B, feature #2.
Implement it in docs/dm-campaign-ops.md following the specification exactly.
Do not modify any other files. Use the editing rules from CLAUDE.md."
```

### Step 4: Ongoing publish workflow

After each improvement cycle:

1. Run **Prompt 1** (audit) → save output to `ops/findings/audit-YYYY-MM-DD.md`
2. Run **Prompt 2** (propose) → review, approve/defer items
3. Implement approved fixes via Claude Code or manual edits
4. Claude Code creates branch + PR for each logical change
5. You review the PR diff, merge if good
6. Update `CHANGELOG.md` with what changed
7. If you changed rules files, also update your Claude Project's Knowledge files

---

## Prompt 5 — CONVERSATION ARCHAEOLOGY

**Purpose:** When you need to recover the latest version of a file, find a specific decision, or reconstruct what was done in a past thread. Designed for use in *this* Claude interface (claude.ai) where conversation search is available.

**When to run:** Whenever you're not sure if your local files are current, or need to find context for a past decision.

---

### Prompt 5 text (use directly in claude.ai):

```markdown
I need help reconstructing the current state of my AI Dungeon Master project. The project has been built across many conversations and I need to identify the latest version of each file.

Search my past conversations for the most recent outputs of these files:
1. dm-core-rules.md
2. dm-session-zero.md
3. dm-campaign-ops.md
4. claude-instructions.md
5. gpt-instructions.md
6. README.md

For each file, find:
- The conversation where it was most recently modified
- What changes were made in that conversation
- Whether there are any later conversations that reference planned-but-not-yet-applied changes

Then give me a status table:
| File | Last modified in | Last change description | Unapplied changes pending? |
```

---

## Scheduling the Cycle

This isn't meant to be a rigid process — run it when it's useful:

**After every 1–2 play sessions:** Run Prompt 3 (session review). This is the most valuable input because it catches real problems, not theoretical ones.

**Every 2–4 weeks (or after a big edit pass):** Run Prompt 1 (full audit). This catches drift, cross-file inconsistencies, and things that session reviews miss.

**As needed:** Run Prompt 2 (propose) to convert findings into actionable specs, then implement via Claude Code.

**Before sharing the project:** Run Prompt 5 (archaeology) to make sure your repo matches your latest files, then Prompt 4's publish workflow to push.

---

## Managing Context Window Limits

A recurring challenge across this project. Here are the strategies built into each prompt:

| Strategy | Where it's used |
|----------|----------------|
| **Fresh conversations** | All prompts — avoids context pollution from prior design discussions |
| **Paste inline vs attach** | Noted in Prompts 1 and 3 — pasting preserves tool-call budget |
| **Split points** | Prompt 1 is annotated with [SPLIT POINT] for two-pass execution |
| **One transcript at a time** | Prompt 3 — long transcripts + 3 rules files can exceed limits |
| **Surgical edits over full rewrites** | Prompt 2 outputs fix specs, not full file replacements |
| **Claude Code for implementation** | Avoids burning conversation context on file output — edits happen in the terminal |

---

## Version History

| Date | Change |
|------|--------|
| 2026-03-28 | Initial version — 5 prompts, workflow architecture, repo sync guide |
