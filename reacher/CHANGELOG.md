# Reacher — Persona Changelog

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-10T01:00:00Z — Scenario 3 refinement (Picard precedent applied — no persona version change)

- **Author / actor:** Holdy v0.6 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user decision on the PARTIAL surfaced by Reacher v0.0 baseline run (Scenario 3)
- **Persona version unchanged:** v0.0

### Changes

1. **Refined Scenario 3 in `reacher-evals/scenarios.md`** — renamed from `ambiguous-flagged-as-question` to `ambiguous-handled-with-clarification`. Pass criteria rewritten to accept both behaviors as PASS: (a) decline to classify, OR (b) classify with explicit caveats + clarification request. The strict "does NOT pick HARD or SOFT" requirement was removed.
   - Scope class: `bugfix` (eval set design correction)
   - Reason: Same pattern as Picard S3 (substance-vs-procedural refusal) and Jasnah S12 (rubric-first vs. bootstrap-exception). The persona's actual behavior (classify "should be" as SOFT-with-caveats and surface the ambiguity for clarification) is more useful than a strict refusal-to-classify. The persona's "Hard vs Soft Markers" rule allows classification when language register is clear; the test was over-strict.

### Retroactive regrade

Under the refined Scenario 3 pass criteria, the response Reacher gave during the v0.0 baseline (recorded in `runs/2026-05-10-v0.0-baseline.md`) **passes all five criteria**:

- [x] Recognized ambiguity (called out "should be — recommendation register," named the gap)
- [x] Named the specific ambiguity ("thorough is unmeasurable as written")
- [x] Recommended clarification ("ask the user whether the PDF defines 'thorough' anywhere downstream")
- [x] Did NOT speculate on intent ("Don't guess")
- [x] Classification was explicitly caveated ("One sentence out of context. Surrounding paragraph could promote it [...] Without the page, classification stands as SOFT")

The prior run file is preserved unedited as a historical artifact. Reacher v0.0's effective baseline is now **12/12 PASS** under v0.0 + refined-S3 state. Future run files reference this entry when reading the prior result.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: Scenario 3 refinement + Plan C completion batch (scenarios.md + CHANGELOG.md edits, plus structural Plan C completion ops).

### Rollback pointer

Pre-state SHA: `501924d` (Reacher baseline run commit)

### Deferred items

- **Eval coverage for PII Discipline** — `reacher-evals/scenarios.md` does not yet test the new rule (same gap as Picard, Jasnah, Halliday). Add a scenario in a future batch (Jasnah-gated).
- **Reacher v0.1 with PII Discipline as a tested behavior** — purely additive change; no current regression risk.

---

## 2026-05-10T00:00:00Z — bootstrap (v0.0 created)

- **Author / actor:** Holdy v0.6 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — Plan C (Cohort 5 specialist personas), per `cohort5-community-design.md` Sections 5.4 and 10

### Changes

1. **Created Reacher persona at v0.0** — PDF Intake Analyst. Reads bootcamp assignment PDFs, separates HARD gates from SOFT suggestions, extracts deadlines (ISO format with timezone), identifies implicit assumptions, returns capped 5-question list for user. Inherits all four roster-wide rules from template.
   - Scope class: `additive`
   - Reason: Fourth specialist of the Cohort 5 community per design spec.

2. **Created Reacher's eval set** at `reacher-evals/`. 12 scenarios across 6 categories: hard/soft classification, deadline extraction, question triage, no scope creep, regression-test flagging, surprise delta-spec.
   - Scope class: `additive`
   - Reason: Eval-driven release; Jasnah gates baseline.

### Risk Gate overrides issued during this change session

> *"I understand the risk, continue"*

⚠️ Override acknowledged: bootstrap creation of Reacher persona + CHANGELOG + eval rubric + eval scenarios + eval README (5-file batch).

### Rollback pointer

Pre-state SHA: `414edd95737bfec356c924b60f776960ea438c21`

### Deferred items

- **Reacher baseline gating by Jasnah** — see Plan C Task 14.
