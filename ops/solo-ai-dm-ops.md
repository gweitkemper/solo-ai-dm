# Solo AI DM — Continuous Improvement Operations

A GitHub-native system for reviewing, improving, and maintaining the solo-ai-dm project.
Uses Claude Code GitHub Actions to run audits, propose fixes, and implement approved changes —
all through GitHub Issues and PRs with you as the approval gate.

---

## How It Works

```
┌──────────────────────────────────────────────────────────────────┐
│                    AUTOMATED IMPROVEMENT CYCLE                   │
│                                                                  │
│  ┌──────────┐    ┌──────────────┐    ┌──────────┐    ┌───────┐  │
│  │  AUDIT   │───►│ FINDINGS     │───►│ YOU      │───►│  PR   │  │
│  │ (Action) │    │ (GH Issue)   │    │ APPROVE  │    │ (Auto)│  │
│  └──────────┘    └──────────────┘    └──────────┘    └───────┘  │
│  Trigger:         Claude posts       You comment:     Claude     │
│  - Schedule       structured         @claude fix #1   branches,  │
│  - Label          findings as        and feat #3      edits,     │
│  - Manual         an Issue                            opens PR   │
│                                                           │      │
│                                                     ┌─────▼──┐  │
│                                                     │ YOU    │  │
│                                                     │ MERGE  │  │
│                                                     └────────┘  │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐    │
│  │ SESSION REVIEW (manual — paste prompt in claude.ai)      │    │
│  │ Feeds findings back into the next audit cycle            │    │
│  └──────────────────────────────────────────────────────────┘    │
└──────────────────────────────────────────────────────────────────┘
```

**What's automated:** Audit → Findings Issue → Implementation → PR creation
**What requires you:** Approving which findings to act on, merging PRs, session reviews

---

## Setup (One Time)

### Prerequisites
- Claude Max subscription (you have this)
- Claude Code CLI installed
- Admin access to the gweitkemper/solo-ai-dm repo

### Step 1: Generate your OAuth token and install the GitHub App

```bash
npm install -g @anthropic-ai/claude-code
cd solo-ai-dm
claude                # make sure you're logged in
claude setup-token    # generates CLAUDE_CODE_OAUTH_TOKEN — copy it
```

Then add the token to your repo:
1. Go to repo Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `CLAUDE_CODE_OAUTH_TOKEN`
4. Value: paste the token from `claude setup-token`

Then install the Claude GitHub App:
```bash
# Still inside Claude Code:
/install-github-app
```
Or manually: install from https://github.com/apps/claude and grant access to `solo-ai-dm`.

### Step 2: Add the workflow files and ops infrastructure

Copy these files into your repo:

```
solo-ai-dm/
├── .github/
│   └── workflows/
│       ├── audit.yml        ← scheduled + on-demand audit
│       └── claude.yml       ← @claude interactive mode
├── CLAUDE.md                ← project rules for Claude Code (updated)
├── CHANGELOG.md             ← version history
├── ops/
│   └── solo-ai-dm-ops.md   ← this file
└── docs/                    ← (already current)
```

```bash
git add -A
git commit -m "feat: add Claude Code GitHub Actions for automated audits and fixes"
git push origin main
```

### Step 3: Test it

Create a new issue in your repo titled "Test audit" and add the label `audit`.
The workflow should trigger within a minute, and Claude will post findings as a
comment on the issue. If it doesn't trigger, check Actions tab for errors — the
most common issue is a missing or incorrect API key secret.

---

## Using the System

### Running an Audit

Three ways to trigger:

| Method | How | When to use |
|--------|-----|-------------|
| **Scheduled** | Automatic — every Monday 9am CT | Ongoing quality baseline |
| **Label** | Create issue, add `audit` label | After a big edit pass or playtest |
| **Manual** | Repo → Actions → "AI DM Audit" → Run workflow | Any time |

The audit reads all files in `docs/`, `README.md`, and `CLAUDE.md`, then posts
structured findings across 10 dimensions: internal contradictions, coverage gaps,
5e rules accuracy, LLM attention risks, cross-file dependency gaps (ChatGPT),
platform parity, README accuracy, token efficiency, worked examples, and
tone/audience fit.

### Approving Changes

Read through the findings issue. Then comment to tell Claude what to implement:

**Approve specific fixes:**
```
@claude Please implement fixes for findings #1, #3, and #7
```

**Approve with extra guidance:**
```
@claude Implement finding #3. The correct STR save modifier for a level 3
Fighter with 16 STR is +5 (+3 mod + 2 prof). Fix it in Example 7.
```

**Approve a feature proposal:**
```
@claude Implement feature opportunity #2. Follow the specification in the
findings. Create it in dm-campaign-ops.md after the Travel section.
```

**Ask for a spec before implementing:**
```
@claude For feature #2, write a detailed specification as a comment here
before implementing. I want to review the design first.
```

**Defer items:**
```
@claude Implement #1 and #7 only. Skip #3, I'll revisit next cycle.
```

Claude will create a branch, apply the changes following the rules in CLAUDE.md,
and open a PR. It never pushes to main.

### Reviewing PRs

For every PR Claude opens:
1. Read the diff — confirm only specified sections changed
2. Check cross-file consistency if a shared term or format was modified
3. Merge if good
4. If rules files changed, update your Claude Project's Knowledge files to match

### Ad-hoc Requests

You can also use `@claude` in any issue or PR comment for one-off tasks:

```
@claude In docs/dm-core-rules.md, the Monk Ki Points entry says
"Short or Long Rest" but Ki recharges on Short Rest only. Fix it.
```

```
@claude Compare docs/claude-instructions.md and docs/gpt-instructions.md.
List any differences that aren't explained by the platform difference.
```

---

## Session Reviews (Manual)

Playtest transcripts need human context and are too long for automated workflows.
After 1–2 play sessions, open a **fresh claude.ai conversation** with this prompt:

**Attach:** session transcript(s) + the 3 rules files from your repo's `docs/` folder

```markdown
You are a QA reviewer for an AI Dungeon Master system (solo D&D 5e).
Read the rules files first, then analyze the transcript.

## Dimensions

### 1. Mechanical accuracy
Check every roll, modifier, damage calc, and resource use against 5e RAW
(2014 PHB) and the stat block in the transcript. For each error: what it says,
what it should say, correct math.

### 2. Rules compliance
- Stat block format matches dm-core-rules.md?
- On-demand command output formatted correctly?
- Session Log present and formatted?
- Choice menus A/B/C/D followed?
- Combat flow (initiative, turns, conditions tracked)?
- Meta-talk channel respected?

### 3. Gaps revealed by play
Situations the rules don't address. Most valuable findings.

### 4. Narrative and engagement quality
NPC voice consistency, meaningful choices, pacing, player agency.

### 5. Character data integrity
Compare end-of-transcript stats against expected values.

## Output
- Mechanical Errors — numbered, with location, issue, correct value
- Rules Compliance — pass/fail checklist
- Gaps Revealed — numbered, with situation, what DM did, what guidance was missing
- Narrative Assessment — 1 honest paragraph
- Character Integrity — stat block comparison
- Summary: quality score (1–10), top 3 working well, top 3 to fix
```

After the review, take the findings and create a GitHub Issue to feed them into
the automated fix cycle. You can just paste the findings into an issue body, then
comment `@claude implement fixes for [items]` to kick off the implementation.

---

## Costs

Covered by your Claude Max subscription — no additional API billing. The workflow
uses an OAuth token that authenticates against your Max plan. Usage counts against
your Max plan's limits, which are generous enough that weekly audits of a ~2,500
line project won't be an issue.

The only cost is GitHub Actions compute minutes (free tier includes 2,000 min/month
for public repos, which is more than enough).

---

## Adjusting the Schedule

The audit runs every Monday at 9am CT by default. To change it, edit the cron
expression in `.github/workflows/audit.yml`:

```yaml
schedule:
  - cron: '0 14 1,15 * *'  # 1st and 15th of each month at 9am CT
```

To disable scheduled runs entirely, remove the `schedule` trigger and keep only
`issues` and `workflow_dispatch`.

---

## Fallback: Manual Prompts

If you'd rather run an audit manually for any reason, the audit prompt is embedded
in `audit.yml` — copy it out and paste into a fresh claude.ai conversation with
files attached. The session review prompt above is already designed for manual use.

---

## Version History

| Date | Change |
|------|--------|
| 2026-03-28 | Initial version — GitHub Actions automation, session review prompt |
