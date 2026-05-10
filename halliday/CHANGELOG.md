# Halliday — Persona Changelog

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

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
