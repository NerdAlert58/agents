# Cohort 5 Plan A — Foundation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
>
> **PERSONA-EDIT CONSTRAINT:** Tasks marked `[main session only]` involve writes to persona-class files (`_templates/system-prompt-template.md`). Per Holdy v0.5 Persona-Edit Authority, subagents cannot perform these writes — the subagent must return; the main session (Holdy) gets user permission + Risk Gate override; main session executes the edit.

**Goal:** Establish the foundation for the Cohort 5 agent community: add Context Discipline + Privacy Boundary as mandatory roster-wide rules in the template, bootstrap the private knowledge base, bootstrap the cohort index.

**Architecture:** Two persona-class edits to `_templates/system-prompt-template.md` (so every persona built from the template inherits the new rules). Two filesystem bootstraps (private KB outside git, cohort index inside git). Single CHANGELOG entry covering the template changes.

**Tech Stack:** Markdown files, git, bash. No code.

**Plan in series:** A → B → C → D. This is Plan A. After completion, write Plan B (Picard + Jasnah personas).

**Success criteria for Plan A:**
- `_templates/system-prompt-template.md` contains Context Discipline + Privacy Boundary sections, bumped version
- `_templates/CHANGELOG.md` exists and records the change
- `~/Desktop/Gauntlet/KnowledgeBase/` exists with README.md and KB.md skeleton
- `~/Git/agents/_planning/cohort5/README.md` exists as cohort index
- All git changes committed to master and pushed to GitHub
- Risk Gate override logged verbatim in template CHANGELOG entry

---

## File Structure

| File | Action | Purpose |
|---|---|---|
| `~/Git/agents/_templates/system-prompt-template.md` | Modify | Add Context Discipline + Privacy Boundary sections under Hard Constraints |
| `~/Git/agents/_templates/CHANGELOG.md` | Create | Track template-level changes (separate from per-agent CHANGELOGs) |
| `~/Desktop/Gauntlet/KnowledgeBase/` | Create directory | Private KB root (outside git) |
| `~/Desktop/Gauntlet/KnowledgeBase/README.md` | Create | Explain what the KB is and how to use it |
| `~/Desktop/Gauntlet/KnowledgeBase/KB.md` | Create | Empty running notebook with structure |
| `~/Git/agents/_planning/cohort5/README.md` | Create | Cohort index — pointers to working repos, KB location, design docs |

---

## Task 1: Read current template (refresh state)

**Subagent allowed:** yes
**Files:**
- Read: `~/Git/agents/_templates/system-prompt-template.md`

- [ ] **Step 1: Read the file**

Run: Use Read tool on `/Users/nerd/Git/agents/_templates/system-prompt-template.md`

Expected: file contents visible. Note the current structure — frontmatter, version comment, sections through Hard Constraints (which currently has Safe File Operations + a placeholder `[Constraint Name]`).

This step exists so the subsequent Edit anchors are guaranteed to match. No commit needed.

---

## Task 2: Add Context Discipline section to template `[main session only]`

**Subagent allowed:** NO — persona-class write requires Persona-Edit Authority + Risk Gate override
**Files:**
- Modify: `~/Git/agents/_templates/system-prompt-template.md` — insert new section under Hard Constraints, between Safe File Operations and the placeholder `[Constraint Name]`

- [ ] **Step 1: Verify Persona-Edit Authority + Risk Gate clearance**

Confirm with user that this batch is approved. The user's prior approval of the spec does NOT count as per-change permission.

Required from user:
- Explicit permission for THIS specific change (or a batch covering Tasks 2 + 3 together)
- Risk Gate override phrase: *"I understand the risk, proceed"* or equivalent

Log the verbatim override quote — it goes in the CHANGELOG entry (Task 4).

- [ ] **Step 2: Apply the edit**

Use Edit tool with anchor that locates the position right after the Safe File Operations section and before whatever comes next.

The section to insert:

```markdown

### Context Discipline
Operate as a slim coordinator or specialist. Push detail to files; keep conversation lean.

- **State lives in files, not conversation.** Maintain structured state files (e.g., `_agents/STATE.md`) for any multi-step work. Reference them by path; do not duplicate their contents in chat.
- **Slim dispatches.** When dispatching a subagent, include only the specific task, the minimum context the specialist needs (paths to relevant artifacts — not their contents), and the expected output shape and location.
- **Summary returns.** When a specialist returns, capture only: result (passed / failed / partial), key decisions, blockers, paths to artifacts produced. Three to five lines.
- **Reference, don't quote.** Refer to files by path. Do not paste file contents into chat unless explicitly necessary.
- **Phase-end checkpoints.** After significant phases, write a brief (~200 word) summary to the relevant state/learnings file. This becomes the recoverable point if the conversation needs to be re-bootstrapped from scratch.
- **Compaction trigger.** When conversation length approaches limits (rough heuristic: ~30K tokens), propose a phase consolidation — write everything to disk and either end the session or compact.

This rule is mandatory across all agents in this roster — do not remove or weaken it.
```

- [ ] **Step 3: Verify the edit landed**

The Edit tool will report success or failure. If failure (anchor mismatch), re-Read the file and try again with a corrected anchor.

No commit yet — Task 3 also edits this file.

---

## Task 3: Add Privacy Boundary section to template `[main session only]`

**Subagent allowed:** NO — same persona-class constraint
**Files:**
- Modify: `~/Git/agents/_templates/system-prompt-template.md` — insert new section under Hard Constraints, after the just-added Context Discipline

- [ ] **Step 1: Confirm Risk Gate override still applies**

If Tasks 2 and 3 were batched in one user permission round, no new override needed. If they were granted separately, get a fresh override now.

- [ ] **Step 2: Apply the edit**

The section to insert (immediately after Context Discipline):

```markdown

### Privacy Boundary
Files under `~/Desktop/Gauntlet/KnowledgeBase/` (and any other path the user marks as private) are personal artifacts. Never copy, paste, summarize, or reference their contents into any file that is tracked by git or destined for a remote repository. Reading from these files is allowed for context; emitting them anywhere else is not.

This rule is mandatory across all agents in this roster — do not remove or weaken it.
```

- [ ] **Step 3: Verify the edit landed**

Same as Task 2. If failure, re-Read and retry.

---

## Task 4: Create `_templates/CHANGELOG.md` and write template-change entry

**Subagent allowed:** yes (the CHANGELOG is not itself a persona file)
**Files:**
- Create: `~/Git/agents/_templates/CHANGELOG.md`

- [ ] **Step 1: Create the CHANGELOG file with header**

Use Write tool to create the file with this content:

```markdown
# Templates — Change Log

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

This log tracks changes to files in `_templates/`. Per-agent CHANGELOG.md files track changes to individual agents' personas; this file tracks changes to the *template* that all future agents inherit.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-09T00:00:00Z — template foundation: add Context Discipline + Privacy Boundary

- **Author / actor:** Holdy v0.5 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — Plan A (Cohort 5 community foundation), per cohort5-community-design.md Section 8

### Changes

1. **Added Context Discipline section** under Hard Constraints in `system-prompt-template.md`. Mandates slim coordination behavior across all roster agents: state in files, slim dispatches, summary returns, reference-not-quote, phase checkpoints, compaction trigger.
   - Scope class: `behavioral` (template-level — flows into all future agents)
   - Reason: Bootcamp will run 8 weeks; agents that bloat conversations break under their own history. Codifying discipline at template level guarantees inheritance.

2. **Added Privacy Boundary section** under Hard Constraints in `system-prompt-template.md`. Mandates that files under `~/Desktop/Gauntlet/KnowledgeBase/` (or user-marked private paths) never enter git-tracked artifacts.
   - Scope class: `behavioral` (template-level)
   - Reason: User established a private knowledge base outside git for personal reflection. Roster-wide rule prevents accidental leakage.

### Risk Gate overrides issued during this change session

> *"<USER_OVERRIDE_VERBATIM_HERE>"*

⚠️ Override acknowledged: behavioral edits to template Hard Constraints (Context Discipline + Privacy Boundary additions)

### Rollback pointer

Pre-state SHA: `<RUN: git rev-parse HEAD BEFORE COMMITTING THIS BATCH>`

### Deferred items

- Holdy v0.5 itself does not yet contain Context Discipline or Privacy Boundary sections — Holdy was built before these were defined. Decide whether to backfill into Holdy or leave Holdy at v0.5. Recommended: backfill in a separate batch when convenient (low priority — Holdy doesn't currently work in the bootcamp lifecycle).
```

**Important:** before committing, replace `<USER_OVERRIDE_VERBATIM_HERE>` with the actual override quote from Task 2 Step 1, and replace `<RUN: git rev-parse HEAD BEFORE COMMITTING THIS BATCH>` with the actual SHA before this commit.

- [ ] **Step 2: Capture the pre-commit SHA**

Run: `cd /Users/nerd/Git/agents && git rev-parse HEAD`
Expected: a SHA (something like `268caa2...`)

Update the CHANGELOG entry's "Rollback pointer" field with this SHA before committing.

---

## Task 5: Commit + push the template changes

**Subagent allowed:** yes (git operations don't trigger persona rules)
**Files:**
- Stage: `_templates/system-prompt-template.md`, `_templates/CHANGELOG.md`

- [ ] **Step 1: Stage the changed files**

Run: `cd /Users/nerd/Git/agents && git add _templates/`
Expected: no output (success)

- [ ] **Step 2: Verify staged changes**

Run: `cd /Users/nerd/Git/agents && git status --short`
Expected output:
```
M  _templates/system-prompt-template.md
A  _templates/CHANGELOG.md
```

If anything else appears in the staging area, unstage it (`git reset HEAD <file>`) before committing.

- [ ] **Step 3: Commit**

Run:
```bash
cd /Users/nerd/Git/agents && git commit -q -m "$(cat <<'EOF'
Template: add Context Discipline + Privacy Boundary as roster-wide rules

Per Cohort 5 community design (Section 8 of spec). Both rules are
behavioral additions to the template's Hard Constraints; every
persona built from the template now inherits them.

Context Discipline: state in files, slim dispatches, summary returns,
reference-not-quote, phase checkpoints, compaction trigger. Prevents
conversation bloat across the 8-week bootcamp.

Privacy Boundary: files under ~/Desktop/Gauntlet/KnowledgeBase/ (or
user-marked private paths) never enter git-tracked artifacts.

Risk Gate override issued by user. CHANGELOG entry captures the
verbatim override and rollback SHA.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)"
```

Expected: no output (commit succeeded silently due to `-q`)

- [ ] **Step 4: Verify commit landed**

Run: `cd /Users/nerd/Git/agents && git log --oneline -3`
Expected: top entry is the just-made commit. Capture the new HEAD SHA for reference.

- [ ] **Step 5: Push to GitHub**

Run: `cd /Users/nerd/Git/agents && git push 2>&1 | tail -3`
Expected: `master -> master` line in output. If error, surface to user.

---

## Task 6: Bootstrap the private knowledge base

**Subagent allowed:** yes (filesystem bootstrap; no persona rules apply)
**Files:**
- Create: `~/Desktop/Gauntlet/KnowledgeBase/` (directory)
- Create: `~/Desktop/Gauntlet/KnowledgeBase/README.md`
- Create: `~/Desktop/Gauntlet/KnowledgeBase/KB.md`

- [ ] **Step 1: Create the directory**

Run: `mkdir -p ~/Desktop/Gauntlet/KnowledgeBase`
Expected: no output. Verify with `ls -la ~/Desktop/Gauntlet/KnowledgeBase/` (should show empty directory with `.` and `..`).

- [ ] **Step 2: Write README.md**

Use Write tool to create `/Users/nerd/Desktop/Gauntlet/KnowledgeBase/README.md` with this content:

```markdown
# Gauntlet Knowledge Base — Private

This directory is the user's personal knowledge base for the GauntletAI Cohort 5 bootcamp. **Nothing here is tracked by git.** Per the Privacy Boundary rule (defined in the agents roster template), no agent may copy, paste, summarize, or reference content from this directory into any git-tracked artifact.

## Structure

- `KB.md` — running notebook. Everything goes here initially. Refactor into topic-indexed subfolders (`concepts/`, `struggles/`) only when KB.md becomes unwieldy.
- `weekN/LEARNINGS.md` — week-specific reflection. Captures: what surprised me this week, what I'd do differently, what concept clicked.

## Intended use

- **End-of-week ritual:** write `weekN/LEARNINGS.md` for the just-completed week
- **Synthesis:** periodically (every 2-3 weeks), read recent LEARNINGS files and update `KB.md` with patterns, concept connections, recurring struggles
- **Reference:** when a new assignment touches a topic you've struggled with before, the KB is the first place to look

## What does NOT live here

- Bootcamp deliverables (those go in `~/Gauntlet/<repo>/`)
- Coordination artifacts like SPEC.md, STATE.md, dispatch logs (those go in working repo's `_agents/`)
- Cross-agent design docs (those live in `~/Git/agents/_planning/`)

This is personal reflection only.
```

- [ ] **Step 3: Write KB.md skeleton**

Use Write tool to create `/Users/nerd/Desktop/Gauntlet/KnowledgeBase/KB.md` with this content:

```markdown
# Knowledge Base — Cross-Week

A running notebook of patterns, concept connections, and recurring struggles across the 8-week bootcamp. Append; rarely edit. When a section gets large enough to deserve its own file, refactor into a subfolder.

---

## Concepts I'm building

*(Empty — populate as concepts solidify)*

## Struggles and resolutions

*(Empty — populate when a confusion gets resolved)*

## Patterns I'm noticing

*(Empty — populate when a recurring shape emerges across weeks)*

## Decisions I'd make differently in hindsight

*(Empty — populate retrospectively)*

## Cross-week threads

*(Empty — populate when a topic spans multiple weeks)*
```

- [ ] **Step 4: Verify the bootstrap**

Run: `ls -la ~/Desktop/Gauntlet/KnowledgeBase/`
Expected: shows README.md and KB.md as files.

No commit needed — this directory is intentionally outside git.

---

## Task 7: Bootstrap the cohort index

**Subagent allowed:** yes
**Files:**
- Create: `~/Git/agents/_planning/cohort5/README.md`

- [ ] **Step 1: Write cohort README**

Use Write tool to create `/Users/nerd/Git/agents/_planning/cohort5/README.md` with this content:

```markdown
# Cohort 5 — Index

GauntletAI Cohort 5 (Austin Admission Track). 8-week intensive bootcamp.

This directory holds the **planning and coordination** layer for the agent community supporting bootcamp work. Per-week deliverables live in `~/Gauntlet/<repo-name>/`. Personal reflection lives in `~/Desktop/Gauntlet/KnowledgeBase/` (private, never in git).

## Documents

| File | Purpose |
|---|---|
| `cohort5-community-design.md` | Approved design spec for the agent community (the canonical reference) |
| `cohort5-community.md` | Running planning notes (open questions, decisions log, suggestions) |
| `cohort5-impl-plan-A-foundation.md` | Implementation plan A (template + bootstraps) |
| *(future)* `cohort5-impl-plan-B-spine-personas.md` | Plan B (Picard + Jasnah) |
| *(future)* `cohort5-impl-plan-C-specialist-personas.md` | Plan C (Halliday + Reacher) |
| *(future)* `cohort5-impl-plan-D-playbooks-and-dryrun.md` | Plan D (4 playbooks + first real-use) |
| *(future)* `decisions/` | Cross-week architectural decisions |

## Path constants

- **Agents repo:** `/Users/nerd/Git/agents`
- **Working repos base:** `/Users/nerd/Gauntlet/` (varies per assignment)
- **Assignment PDFs source:** `/Users/nerd/Desktop/Gauntlet/Assignments/`
- **Private knowledge base:** `/Users/nerd/Desktop/Gauntlet/KnowledgeBase/` (NEVER in git)

## Per-week working repo index

| Week | Project name | Working repo path | Status |
|---|---|---|---|
| 1 | AgentForge Clinical Co-Pilot | (not backfilled — see Plan B) | DONE |
| 2 | AgentForge Clinical Co-Pilot — Multimodal | (not backfilled) | DONE |
| 3 | (TBD when PDF drops) | (TBD) | upcoming |
| 4 | (TBD) | (TBD) | future |
| 5 | (TBD) | (TBD) | future |
| 6 | (TBD) | (TBD) | future |
| 7 | (TBD) | (TBD) | future |
| 8 | (TBD) | (TBD) | future |

Update this table when each week's working repo is created.

## Cohort-wide rules in effect

- **Persona-Edit Authority** (only Holdy edits personas; per-change user permission required) — see Holdy v0.5
- **Safe File Operations** (`cp -p src dst && rm src` for moves; never `mv` across filesystems) — roster-wide
- **Context Discipline** (state in files; slim dispatches; reference, don't quote) — roster-wide as of template foundation commit
- **Privacy Boundary** (private KB never enters git) — roster-wide as of template foundation commit
```

- [ ] **Step 2: Stage and commit**

Run:
```bash
cd /Users/nerd/Git/agents && git add _planning/cohort5/README.md && git status --short
```
Expected:
```
A  _planning/cohort5/README.md
```

- [ ] **Step 3: Commit and push**

Run:
```bash
cd /Users/nerd/Git/agents && git commit -q -m "$(cat <<'EOF'
Bootstrap cohort5 index README

Per Plan A. Provides a navigable index for the cohort planning
directory: documents inventory, path constants, per-week repo
table (to be populated as weeks land), and a quick reference
to roster-wide rules in effect.

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)" && git push 2>&1 | tail -3
```
Expected: `master -> master` line.

---

## Task 8: Add Plan A's own implementation plan to git

**Subagent allowed:** yes
**Files:**
- Stage: `_planning/cohort5/cohort5-impl-plan-A-foundation.md` (this file)

- [ ] **Step 1: Stage this plan file**

Run: `cd /Users/nerd/Git/agents && git add _planning/cohort5/cohort5-impl-plan-A-foundation.md`

- [ ] **Step 2: Commit and push**

Run:
```bash
cd /Users/nerd/Git/agents && git commit -q -m "$(cat <<'EOF'
Add Plan A implementation plan (foundation)

First of four sub-plans implementing the Cohort 5 community design.
Plan A scope: template additions (Context Discipline + Privacy
Boundary), private KB bootstrap, cohort index bootstrap.

Subsequent plans: B (Picard + Jasnah), C (Halliday + Reacher),
D (playbooks + dry run).

Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>
EOF
)" && git push 2>&1 | tail -3
```

---

## Task 9: Verify Plan A success criteria

**Subagent allowed:** yes (read-only verification)
**Files:**
- Read: all the artifacts created

- [ ] **Step 1: Verify template now contains both new sections**

Run: `grep -A 1 "### Context Discipline\|### Privacy Boundary" ~/Git/agents/_templates/system-prompt-template.md`

Expected: shows both section headers with their first content line.

- [ ] **Step 2: Verify template CHANGELOG exists and has the entry**

Run: `head -30 ~/Git/agents/_templates/CHANGELOG.md`

Expected: shows the header + the first entry. Verify the entry has all 9 required fields filled in (no `<USER_OVERRIDE_VERBATIM_HERE>` placeholder still present).

- [ ] **Step 3: Verify private KB exists**

Run: `ls -la ~/Desktop/Gauntlet/KnowledgeBase/`

Expected: shows `README.md` and `KB.md`.

- [ ] **Step 4: Verify cohort index exists**

Run: `ls -la ~/Git/agents/_planning/cohort5/`

Expected: shows `README.md`, `cohort5-community-design.md`, `cohort5-community.md`, `cohort5-impl-plan-A-foundation.md`.

- [ ] **Step 5: Verify all commits pushed**

Run: `cd /Users/nerd/Git/agents && git status && git log --oneline -5`

Expected: working tree clean, recent commits include the three Plan A commits (template, cohort index, plan file).

- [ ] **Step 6: Report Plan A complete**

Plan A success criteria all met:
- ✅ Template contains Context Discipline + Privacy Boundary
- ✅ Template CHANGELOG records the change with override quote and SHA
- ✅ Private KB bootstrapped (README + KB skeleton)
- ✅ Cohort index bootstrapped
- ✅ All commits pushed to GitHub
- ✅ Risk Gate override logged verbatim

Plan A complete. Ready for Plan B (Picard + Jasnah personas).
