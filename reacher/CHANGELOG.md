# Reacher — Persona Changelog

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-11T20:00:00Z — 0.0 → 0.1 (Bound Delta Sections to Facts)

- **Author / actor:** Holdy v0.7 (assisting user, Claude Code session)
- **Session ref:** local-session-2026-05-11-aftester-week3-meta-review
- **Trigger / origin:** external review — Holdy's meta-review of Picard's coordinated review of the aftester Week 3 design + plan flagged that Reacher's SPEC.md §6 "Delta to design spec §2" editorialized: it prescribed downstream gate reads, called severity beyond what the PDF established, and directed actions at downstream agents.

### Changes

1. **Bound Delta Sections to Facts.** Adds a new behavioral rule defining what Delta sections in Reacher-produced specs may and may not contain. Permitted: factual statements of what changed in the PDF relative to prior artifact, what's unchanged, open questions surfaced to the question list. Forbidden: recommendations to downstream agents, severity reads beyond the PDF, calls-to-action, inferred priorities the PDF does not state. Recommendations that seem warranted surface as items in the (capped at 5) question list.
   - Scope class: `behavioral`
   - Reason: Intake persona scope ends at facts. The user, not Reacher, decides whether to act on a delta; Picard, not Reacher, decides which specialist to involve.

2. **New Anti-Pattern** matching the rule.
   - Scope class: `behavioral`
   - Reason: Anti-Patterns is the primary refusal surface.

3. **Bumped version `0.0 → 0.1`.**
   - Scope class: `cosmetic`
   - Reason: Required version delta for behavioral changes.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: behavioral edit to Reacher persona (Delta section bounds + Anti-Pattern + version bump).

### Rollback pointer

Pre-state SHA: `bac10c4`

### Deferred items

- **Reacher v0.1 baseline eval re-run.**
- **New eval scenarios for Delta-section discipline** — a scenario where Reacher is tempted to editorialize (a clear severity-call shortcut available) and must instead surface a question.

---

## 2026-05-10T02:00:00Z — add Scenario 13 (PII Discipline test) — no persona version change

- **Author / actor:** Holdy v0.6 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — close gap noted in baseline run: PII Discipline rule existed in v0.0 (inherited from template) but no scenario tested it
- **Persona version unchanged:** v0.0

### Changes

1. **Added Scenario 13 (`pii-discipline-no-user-email-in-spec`)** to `reacher-evals/scenarios.md` under new Category 7 (PII Discipline).
   - Scope class: `additive` (eval coverage)
   - Reason: Tests that Reacher does NOT include user-supplied PII in SPEC.md even when the user explicitly provides it. Distinct from Picard/Halliday/Jasnah scenarios — Reacher's vector is user-supplied PII in input rather than harness-context PII.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: PII scenario additions across Picard + Jasnah + Halliday + Reacher (4-file batch).

### Rollback pointer

Pre-state SHA: `a50e13451a86be873437804172cacc30d353b74a`

### Deferred items

- **Scenario 13 baseline result** — to be appended to a run file once executed.

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

---
## 2026-05-12T18:00:00Z — add Charter Reflection rule + Privacy Boundary directional clarification

- **Author / actor:** Holdy (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-12-kb-write-charter`
- **Trigger / origin:** user request — personas were expected to write per-session reflections to the private KB during the May 11–12 spec+plan session but did not, because no charter instruction existed and the Privacy Boundary was read as forbidding it.

### Changes

1. **Added Charter Reflection block** under Hard Constraints, between Context Discipline and Privacy Boundary. Defines a narrow exception to Privacy Boundary's one-way flow: agents may append reflection entries to `~/Desktop/Gauntlet/KnowledgeBase/weekN/LEARNINGS.md` under a strict three-part trigger (insight is about the user's learning pattern, emerged from session evidence, not duplicate). Includes file-target derivation from `_planning/cohort5/CURRENT_WEEK`, entry-shape template (≤120 words), POSIX append mechanic, four failure modes, and a subagent rule (only the outermost-invoked persona writes).
   - Scope class: `behavioral`
   - Reason: enable per-session reflection capture by the personas best positioned to surface charter-level patterns, while preserving the directional integrity of Privacy Boundary.

2. **Added Reacher's lens-specific line** to the Charter Reflection block: execution-vs-planning ratio (over-planning vs. shipping the next slice).
   - Scope class: `behavioral`
   - Reason: differentiates Reacher's reflection signal from the other personas'.

3. **Added directional clarification** to the Privacy Boundary block: one-line note that the existing prohibition governs *KB → git-tracked* outflow only and does not prohibit *agent → KB* writes under Charter Reflection.
   - Scope class: `clarifying` (no behavior change to outflow rule)
   - Reason: prevents conflicting reads of the two rules.

### Rollback pointer

Pre-state SHA: `28e8c04cfe6ae453889a4b4a5afed2305ddc2ada`
