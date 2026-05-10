# Cohort 5 Plan D — Playbooks + Dry Run Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development.
>
> **PERSONA-EDIT CONSTRAINT:** Playbook files in `_playbooks/` are agent-definition-class (they govern general-purpose subagent behavior). Tasks marked `[main session only]` require Risk Gate override.

**Goal:** Build the four initial playbooks (audit, user-definition, cost-analysis, interview-prep), validate each is well-formed, and document the real-use dry run pattern (the dry run itself waits for Week 3 to drop).

**Architecture:** Five new files in `_playbooks/`: README.md + 4 playbook markdown files. Each playbook follows the format defined in the community design spec Section 6: Purpose, When to invoke, Inputs, Process, Outputs, Pass criteria (boolean for Jasnah), Common pitfalls, Promotion notes. Validated via Jasnah dispatched per playbook to verify completeness and followability.

**Tech Stack:** Markdown files, git, bash, Claude Code subagent dispatch.

**Plan in series:** A → B → C → D. Plans A, B, C complete (commits `3215ddb` through `e61c065`). This is Plan D — final initial plan.

**Success criteria:**
- `_playbooks/` directory exists with README + 4 playbooks
- Each playbook has all required sections per design spec format
- Each playbook validated by Jasnah for completeness and followability (4 Jasnah dispatches; all PASS)
- Cohort README updated with playbook roster
- All committed and pushed
- Real-use dry run documented as deferred to Week 3 drop, with a clear "what to do when the PDF lands" runbook

**Out of scope for Plan D:**
- Real-use dry run on actual bootcamp data (waits for Week 3 PDF)
- Knowledge curation playbook (deferred per design spec; expected promotion to persona pre-Week 4)
- Demo direction, security review, documentation playbooks (deferred per design spec)

---

## File Structure

| File | Action |
|---|---|
| `~/Git/agents/_playbooks/` | Create directory |
| `~/Git/agents/_playbooks/README.md` | Create |
| `~/Git/agents/_playbooks/audit.md` | Create |
| `~/Git/agents/_playbooks/user-definition.md` | Create |
| `~/Git/agents/_playbooks/cost-analysis.md` | Create |
| `~/Git/agents/_playbooks/interview-prep.md` | Create |
| `~/Git/agents/_planning/cohort5/README.md` | Modify (add playbook roster section + Week 3 runbook) |

---

## Risk Gate strategy

**Single override** covers the 5 playbook file creations + cohort README update. All persona/agent-definition class.

---

## Task 1: Create `_playbooks/` directory

**Subagent allowed:** yes
- [ ] `mkdir -p /Users/nerd/Git/agents/_playbooks`

---

## Task 2: Risk Gate request `[main session only]`

- [ ] Get user permission + Risk Gate override for the 5-file batch (4 playbooks + README) + cohort README update
- [ ] Capture pre-batch SHA

---

## Task 3: Write `_playbooks/README.md` `[main session only]`

Standard format introducing the playbook roster. Single Write.

## Task 4: Write `_playbooks/audit.md` `[main session only]`

Per design spec Section 6 format. Process produces `AUDIT.md` covering 5 required categories (Security, Performance, Architecture, Data Quality, Compliance) with 500-word summary leading. Pass criteria boolean for Jasnah.

## Task 5: Write `_playbooks/user-definition.md` `[main session only]`

Process produces `USERS.md` with narrow primary user, workflow context, concrete use cases, advisory-vs-actionable line. Pass criteria boolean.

## Task 6: Write `_playbooks/cost-analysis.md` `[main session only]`

Process produces `AI_COST_ANALYSIS.md` with per-session cost, projections at 100/1K/10K/100K users, architectural-change notes per tier. Pass criteria boolean.

## Task 7: Write `_playbooks/interview-prep.md` `[main session only]`

Process produces `_agents/INTERVIEW-PREP.md` with hostile-question drills, weak points, defense narrative. Pass criteria boolean.

## Task 8: Update cohort README `[main session only]`

Add a "Playbooks" section listing the 4 initial playbooks + a "Week 3 Runbook" that documents the exact dispatch sequence Picard will follow when the Week 3 PDF drops.

---

## Task 9: Validate each playbook via Jasnah

**Subagent allowed:** yes (4 Jasnah dispatches, one per playbook)

For each playbook, dispatch Jasnah with:
- Path to the playbook file
- Instruction to grade against a meta-rubric: does the playbook have all required sections per design spec format? Are pass criteria boolean? Is the process followable by a fresh general-purpose subagent?

Jasnah's verdicts: PASS / FAIL / PARTIAL per playbook.

**Decision gate:** All 4 must PASS before Plan D is complete. If any FAIL, fix and re-validate.

---

## Task 10: Commit + push

Stage all new files + updates. Single commit.

---

## Task 11: Verify Plan D success criteria

- [ ] `_playbooks/` exists with 5 files
- [ ] Each playbook has Purpose, When to invoke, Inputs, Process, Outputs, Pass criteria, Common pitfalls, Promotion notes
- [ ] All 4 playbooks Jasnah-validated
- [ ] Cohort README has playbook roster + Week 3 runbook
- [ ] All committed and pushed

## Task 12: Document deferred items

The real-use dry run requires a Week 3 PDF. Once Week 3 drops:

1. User tells Picard the working repo path for Week 3
2. Picard creates `_agents/` in the working repo, seeds STATE.md
3. Picard dispatches Reacher with the PDF
4. Reacher produces SPEC.md + question list
5. User answers the questions
6. Picard dispatches general-purpose with `_playbooks/audit.md`
7. Picard dispatches general-purpose with `_playbooks/user-definition.md`
8. Picard dispatches Halliday with USERS.md + AUDIT.md to produce ARCHITECTURE.md
9. Picard dispatches Jasnah to gate the architecture
10. Picard dispatches general-purpose with `_playbooks/cost-analysis.md` later in the week
11. Picard dispatches general-purpose with `_playbooks/interview-prep.md` before each AI Interview deadline

This becomes the canonical workflow. Deviations and gaps surface naturally during the first real run.

---

## Plan D complete

After Plan D: full Cohort 5 community is operational. 5 dispatchable agents + 4 playbooks + Week 3 runbook + Eval Grading Policy + Privacy/PII discipline rules. Ready for the bootcamp's Week 3 drop.
