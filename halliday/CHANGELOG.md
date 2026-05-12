# Halliday — Persona Changelog

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-11T20:00:00Z — 0.0 → 0.1 (artifact-as-claimed + closure propagation)

- **Author / actor:** Holdy v0.7 (assisting user, Claude Code session)
- **Session ref:** local-session-2026-05-11-aftester-week3-meta-review
- **Trigger / origin:** external review — Holdy's meta-review of Picard's coordinated review of the aftester Week 3 design + plan surfaced two reviewer-side gaps: template-grading of artifacts with narrower claimed scope, and findings tracked but not decomposed at close-time.

### Changes

1. **Grade Artifact-As-Claimed, Not Artifact-As-Template.** When reviewing an existing artifact, Halliday must capture `claimed_scope:` before applying the three filters; the filters then operate against that scope, not against a canonical template. If claimed scope is absent or ambiguous, Halliday returns BLOCKED with "claimed_scope undetermined."
   - Scope class: `behavioral`
   - Reason: A correct finding against the wrong scope is still a wrong finding. Pass-1 review surfaced several technically-correct findings that were contextually invalid because the artifact claimed MVP-narrow scope.

2. **Closure Propagation.** When marking a finding closed, Halliday must enumerate `closure_artifacts:` (verified updated) and `unverified:` (should have been checked). Closures with non-empty `unverified:` return as PARTIAL CLOSURE, not CLOSED.
   - Scope class: `behavioral`
   - Reason: Findings tracked but not decomposed at close-time are the source of residual issues discovered late (4 residual items surfaced in Pass-3 that were within Halliday's grain and should have been caught in Pass-2).

3. **Two new Anti-Patterns** matching the above rules.
   - Scope class: `behavioral`
   - Reason: Anti-Patterns is the surface most likely to fire on review-mode dispatches.

4. **Bumped version `0.0 → 0.1`.**
   - Scope class: `cosmetic`
   - Reason: Required version delta for behavioral changes.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: behavioral edits to Halliday persona (claimed-scope + closure-propagation + Anti-Patterns + version bump).

### Rollback pointer

Pre-state SHA: `bac10c4`

### Deferred items

- **Halliday v0.1 baseline eval re-run.**
- **New eval scenarios for review-mode dispatches** — Halliday's current eval set is producer-side; reviewer-side scenarios should be added.

---

## 2026-05-10T02:00:00Z — add Scenario 13 (PII Discipline test) — no persona version change

- **Author / actor:** Holdy v0.6 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — close gap noted in baseline run: PII Discipline rule existed in v0.0 (inherited from template) but no scenario tested it
- **Persona version unchanged:** v0.0

### Changes

1. **Added Scenario 13 (`pii-discipline-opaque-attribution`)** to `halliday-evals/scenarios.md` under new Category 7 (PII Discipline).
   - Scope class: `additive` (eval coverage)
   - Reason: Tests that Halliday uses opaque attribution in ARCHITECTURE.md, never pulling user PII from harness context — even when explicitly asked to.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: PII scenario additions across Picard + Jasnah + Halliday + Reacher (4-file batch).

### Rollback pointer

Pre-state SHA: `a50e13451a86be873437804172cacc30d353b74a`

### Deferred items

- **Scenario 13 baseline result** — to be appended to a run file once executed.

---

## 2026-05-09T02:00:00Z — bootstrap (v0.0 created)

- **Author / actor:** Holdy v0.6 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — Plan C (Cohort 5 specialist personas), per `cohort5-community-design.md` Sections 5.3 and 10

### Changes

1. **Created Halliday persona at v0.0** — System Architect for bootcamp assignments. Refuses to design without `USERS.md` and `AUDIT.md`. Failure-modes-first; trust boundaries explicit; 500-word summary mandatory. Inherits all four roster-wide rules from template (Safe File Operations, Context Discipline, Privacy Boundary, PII Discipline).
   - Scope class: `additive`
   - Reason: Third specialist of the Cohort 5 community per design spec.

2. **Created Halliday's eval set** at `halliday-evals/`. 12 scenarios across 6 categories: refuse-without-inputs (USERS/AUDIT), trust-boundaries-mapped, failure-modes-first, trade-offs-cited, scale-analysis, surprise-delta-architecture.
   - Scope class: `additive`
   - Reason: Eval-driven release; Jasnah gates baseline.

### Risk Gate overrides issued during this change session

> *"I accept the risk, proceed"*

⚠️ Override acknowledged: bootstrap creation of Halliday persona + CHANGELOG + eval rubric + eval scenarios + eval README (5-file batch).

### Rollback pointer

Pre-state SHA: `92c723a0cb75189b060142c3a3b2d09f7d82b7f3`

### Deferred items

- **Halliday baseline gating by Jasnah** — see Plan C Tasks 7.

---
## 2026-05-12T18:00:00Z — add Charter Reflection rule + Privacy Boundary directional clarification

- **Author / actor:** Holdy (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-12-kb-write-charter`
- **Trigger / origin:** user request — personas were expected to write per-session reflections to the private KB during the May 11–12 spec+plan session but did not, because no charter instruction existed and the Privacy Boundary was read as forbidding it.

### Changes

1. **Added Charter Reflection block** under Hard Constraints, between Context Discipline and Privacy Boundary. Defines a narrow exception to Privacy Boundary's one-way flow: agents may append reflection entries to `~/Desktop/Gauntlet/KnowledgeBase/weekN/LEARNINGS.md` under a strict three-part trigger (insight is about the user's learning pattern, emerged from session evidence, not duplicate). Includes file-target derivation from `_planning/cohort5/CURRENT_WEEK`, entry-shape template (≤120 words), POSIX append mechanic, four failure modes, and a subagent rule (only the outermost-invoked persona writes).
   - Scope class: `behavioral`
   - Reason: enable per-session reflection capture by the personas best positioned to surface charter-level patterns, while preserving the directional integrity of Privacy Boundary.

2. **Added Halliday's lens-specific line** to the Charter Reflection block: requirements-clarity drift (design committed before intent pinned).
   - Scope class: `behavioral`
   - Reason: differentiates Halliday's reflection signal from the other personas'.

3. **Added directional clarification** to the Privacy Boundary block: one-line note that the existing prohibition governs *KB → git-tracked* outflow only and does not prohibit *agent → KB* writes under Charter Reflection.
   - Scope class: `clarifying` (no behavior change to outflow rule)
   - Reason: prevents conflicting reads of the two rules.

### Rollback pointer

Pre-state SHA: `28e8c04cfe6ae453889a4b4a5afed2305ddc2ada`
