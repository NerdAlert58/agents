# Picard — Persona Changelog

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-09T01:00:00Z — 0.0 → 0.1 (PII Discipline backfill)

- **Author / actor:** Holdy v0.5 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — PII Discipline backfill batch following Jasnah's v0.0 baseline eval finding (subagent pulled `userEmail` from harness context into a CHANGELOG-class artifact)

### Changes

1. **Added PII Discipline section** under Hard Constraints in `system-prompt.md`, placed after Privacy Boundary. Mandates that user PII never enters committed artifacts; identity references in logs use opaque markers ("user," "owner," etc.).
   - Scope class: `behavioral`
   - Reason: Picard logs override quotes verbatim into STATE.md (per its own rules). Without PII Discipline, Picard could leak PII from harness context the same way the eval surfaced.

2. **Bumped version `0.0 → 0.1`.**
   - Scope class: `cosmetic`
   - Reason: Required version delta for behavioral change.

### Risk Gate overrides issued during this change session

> *"I get it, proceed"*

⚠️ Override acknowledged: PII Discipline backfill across template + Jasnah + Picard + Holdy (8-file batch under one override).

### Rollback pointer

Pre-state SHA: `f5212fb324bc237771f880a41877f40f4025ad21`

### Deferred items

- **Eval coverage for PII Discipline** — `picard-evals/scenarios.md` does not yet test the new rule. Add a scenario in a future batch (Jasnah-gated).
- **Picard's baseline eval re-run under v0.1** — additive behavioral change should not regress existing scenarios, but a re-run would confirm. Deferred for now since Picard hasn't been baselined yet (eval planned for Plan B Tasks 18-19).

---

## 2026-05-09T00:00:00Z — bootstrap (v0.0 created)

- **Author / actor:** Holdy v0.5 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — Plan B (Cohort 5 spine personas), per `cohort5-community-design.md` Sections 5.1 and 10

### Changes

1. **Created Picard persona at v0.0** — Lead Orchestrator for GauntletAI Cohort 5 assignment work. Five-layer architecture (Identity, Primary Job, Domain Lens, Behavioral Rules, Hard Constraints, Operating Environment, Edge Case Handling). Inherits Safe File Operations, Context Discipline, Privacy Boundary from template. Adds Persona-Edit Authority Deference (only Holdy edits personas) and bootcamp-specific Hard Gate Discipline.
   - Scope class: `additive` (initial build)
   - Reason: First specialist agent of the Cohort 5 community per the approved community design spec.

2. **Created Picard's eval set scaffold** at `picard-evals/`. 12 scenarios across 7 categories: triage protocol, dispatch protocol, handoff protocol, verifier deference, scope refusal (substantive work), hard-gate refusal, two-specialist disagreement, persona-edit authority deference, context discipline.
   - Scope class: `additive`
   - Reason: Eval-driven release per design spec; eval set required before symlink.

### Risk Gate overrides issued during this change session

> *"I understand the risk, proceed"*

⚠️ Override acknowledged: bootstrap creation of Picard persona + CHANGELOG + eval rubric + eval scenarios + eval README (batch).

### Rollback pointer

Pre-state SHA: `3215ddb4cf6e4dd45d08aacb303b94ff9145a94c`

### Deferred items

- **Picard's baseline eval gating** — Picard cannot be symlinked into `~/.claude/agents/` until Jasnah exists and gates Picard's eval baseline. Tracked in Plan B Tasks 18-19.
- **Voice tuning** — TNG cadence appropriateness will be observed during first real session; tweak if it reads as cosplay.
