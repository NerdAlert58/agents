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
- **Context Discipline** (state in files; slim dispatches; reference, don't quote) — roster-wide as of template foundation commit `ecf3798`
- **Privacy Boundary** (private KB never enters git) — roster-wide as of template foundation commit `ecf3798`
- **PII Discipline** (no user PII in committed artifacts; opaque markers only) — roster-wide as of PII Discipline backfill commit `5314132`

## Eval Grading Policy

**Adopted 2026-05-10.** All persona baseline runs (and any subsequent re-runs after persona edits) MUST be graded by Jasnah dispatched as a subagent — not by manual grading from any human or any other agent.

### Why
- **Independence:** the persona designer (Holdy) has a stake in the persona passing. Manual grading is biased.
- **Repeatability:** Jasnah follows the rubric mechanically; manual judgment varies.
- **Slop detection:** Jasnah is specifically tuned for claim-vs-evidence gap detection.
- **Bootcamp parity:** the bootcamp itself demands eval-driven CI with boolean rubrics. Skipping Jasnah is skipping the discipline we built her for.

### How
For each baseline / re-run:
1. Dispatch the persona N times (N = scenario count) to capture verbatim responses.
2. Save responses to `<persona>-evals/runs/<DATE>-vX.Y-baseline-responses.md`.
3. Dispatch Jasnah with three inputs: persona file path (for her own persona), scenarios.md path (for rubric), responses file path (for artifacts under test).
4. Jasnah returns PASS / FAIL / PARTIAL per scenario.
5. Save Jasnah's verdicts in the standard run file (`<DATE>-vX.Y-baseline.md`).
6. Manual grading by Holdy or human is **supplemental only** — never a replacement for Jasnah's verdict.

### Exceptions (very narrow)
- **Jasnah's own baseline** — bootstrap exception, one-time. Holdy + user grade. Documented in Jasnah's CHANGELOG.
- **True emergency** (deadline within minutes, no time for Jasnah dispatch) — manual grade allowed but must be flagged with `[MANUAL-GRADE-EMERGENCY]` in the run file and re-graded by Jasnah within 24 hours.

### Retroactive application
Picard, Halliday, and Reacher's existing baselines were manually graded before this policy was adopted. Each has been re-graded by Jasnah retroactively (see commits around `1f3e3f7`+). Going forward, no exceptions outside those listed above.
