# Playbooks

Codified workflows for general-purpose subagents. Each playbook is a self-contained markdown file that a fresh `general-purpose` subagent can follow without prior context.

## Why playbooks instead of personas

Per the Cohort 5 community design (lean iterative growth): personas are expensive to design, eval, maintain. Playbooks are cheap. We codify recurring workflows as playbooks first; promote a playbook to a persona only when:

1. It's been used in 3+ real assignments
2. The general-purpose output has needed material rework 2+ times
3. The role has stable enough scope to be tightly characterized

When all three fire, Picard surfaces the playbook as a promotion candidate. User decides; if approved, Holdy designs the persona. Playbook moves to `_archived/` with a pointer to the new persona.

## Playbook format

Every playbook has these sections (defined in `_planning/cohort5/cohort5-community-design.md` Section 6):

- **Purpose** — one line
- **When to invoke** — triggers
- **Inputs (required)** — paths to files the playbook expects
- **Inputs (optional)** — paths that help if available
- **Process** — numbered steps the subagent follows
- **Outputs** — exact file path + required structure
- **Pass criteria (boolean rubric for Jasnah)** — checkable bullets
- **Common pitfalls** — what to watch for
- **Promotion notes** — usage tracking for promotion criteria

## Initial roster (Plan D)

| Playbook | Purpose | Picard dispatches when |
|---|---|---|
| `audit.md` | Produce `AUDIT.md` covering 5 categories from a forked codebase | After Reacher's SPEC.md exists; before Halliday's architecture work |
| `user-definition.md` | Produce `USERS.md` — narrow primary user, workflow, use cases | After audit; before Halliday's architecture work |
| `cost-analysis.md` | Produce `AI_COST_ANALYSIS.md` — per-session cost + multi-tier projections | After ARCHITECTURE.md exists; before final submission |
| `interview-prep.md` | Produce `_agents/INTERVIEW-PREP.md` — hostile question drills + defense narrative | Before each AI Interview deadline |

## Dispatch pattern

Picard dispatches a `general-purpose` subagent with a prompt of this shape:

```
Read /Users/nerd/Git/agents/_playbooks/<playbook>.md and follow it exactly.

Inputs:
- <input path 1>
- <input path 2>

Output: write to <output path> per the playbook's Outputs section.

Return a 5-line summary to me (this dispatcher) when complete.
```

The subagent reads the playbook, follows the Process, writes the Output, returns the summary. Picard then dispatches Jasnah to verify against the playbook's Pass criteria.

## Deferred playbooks

Per design spec, write when need is observed:

- `demo-direction.md` — script the 3–5 min demo video
- `security-review.md` — HIPAA + general (may overlap with audit)
- `documentation.md` — meta-playbook for 500-word summaries
- `knowledge-curation.md` — capture week's learnings into private KB. **Persona-promotion candidate from day one** per design spec.

## Archived playbooks

`_archived/` (created on first promotion). When a playbook is promoted to a persona, the playbook file moves here with a header note pointing to the new persona.
