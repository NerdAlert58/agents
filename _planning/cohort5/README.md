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
