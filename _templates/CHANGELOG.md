# Templates — Change Log

Append-only. Never edit prior entries; correct mistakes with a new corrective entry that references the prior one.

This log tracks changes to files in `_templates/`. Per-agent CHANGELOG.md files track changes to individual agents' personas; this file tracks changes to the *template* that all future agents inherit.

Entry format follows the schema defined in `~/Git/agents/holdy/system-prompt.md` → "Change Logging — Mandatory."

---

## 2026-05-09T01:00:00Z — add PII Discipline rule

- **Author / actor:** Holdy v0.5 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — PII Discipline backfill batch following Jasnah's v0.0 baseline eval finding (subagent pulled `userEmail` from harness context into a CHANGELOG-class artifact)

### Changes

1. **Added PII Discipline section** under Hard Constraints in `system-prompt-template.md`, placed after Privacy Boundary. Future agents built from the template inherit the rule. Mandates: never include user PII (email, real names not in design docs, phone, address, govt IDs) in any committed artifact. Identity references in logs use opaque markers ("user," "owner," etc.).
   - Scope class: `behavioral` (template-level — flows into all future agents)
   - Reason: Privacy guard against PII leakage from harness context (`userEmail` etc.) into committed files. Distinct from Privacy Boundary (which covers private KB files); PII Discipline covers PII anywhere in the system.

### Risk Gate overrides issued during this change session

> *"I get it, proceed"*

⚠️ Override acknowledged: PII Discipline backfill across template + Jasnah + Picard + Holdy (8-file batch under one override).

### Rollback pointer

Pre-state SHA: `f5212fb324bc237771f880a41877f40f4025ad21`

### Deferred items

None new. The companion backfills into Jasnah, Picard, and Holdy ship in this same batch.

---

## 2026-05-09T00:00:00Z — template foundation: add Context Discipline + Privacy Boundary

- **Author / actor:** Holdy v0.5 (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-08-holdy-bootstrap` (continued)
- **Trigger / origin:** user request — Plan A (Cohort 5 community foundation), per `cohort5-community-design.md` Section 8

### Changes

1. **Added Context Discipline section** under Hard Constraints in `system-prompt-template.md`. Mandates slim coordination behavior across all roster agents: state in files, slim dispatches, summary returns, reference-not-quote, phase checkpoints, compaction trigger.
   - Scope class: `behavioral` (template-level — flows into all future agents)
   - Reason: Bootcamp will run 8 weeks; agents that bloat conversations break under their own history. Codifying discipline at template level guarantees inheritance.

2. **Added Privacy Boundary section** under Hard Constraints in `system-prompt-template.md`. Mandates that files under `~/Desktop/Gauntlet/KnowledgeBase/` (or user-marked private paths) never enter git-tracked artifacts.
   - Scope class: `behavioral` (template-level)
   - Reason: User established a private knowledge base outside git for personal reflection. Roster-wide rule prevents accidental leakage.

### Risk Gate overrides issued during this change session

> *"I understand the risk, do it"*

⚠️ Override acknowledged: behavioral edits to template Hard Constraints (Context Discipline + Privacy Boundary additions). Both changes batched under a single user permission round per medium-permission convention.

### Rollback pointer

Pre-state SHA: `268caa28682e0483f7ade60404b9a90fd853ab19`

### Deferred items

- Holdy v0.5 itself does not yet contain Context Discipline or Privacy Boundary sections — Holdy was built before these were defined. Decide whether to backfill into Holdy or leave Holdy at v0.5. Recommended: backfill in a separate batch when convenient (low priority — Holdy doesn't currently work in the bootcamp lifecycle directly).

---
## 2026-05-12T18:00:00Z — add Charter Reflection rule + Privacy Boundary directional clarification

- **Author / actor:** Holdy (assisting user, Claude Code session)
- **Session ref:** `local-session-2026-05-12-kb-write-charter`
- **Trigger / origin:** user request — personas were expected to write per-session reflections to the private KB during the May 11–12 spec+plan session but did not. Charter Reflection is added at the template level so future agents inherit the rule.

### Changes

1. **Added Charter Reflection section** under Hard Constraints in `system-prompt-template.md`, placed between Context Discipline and Privacy Boundary. Future agents built from the template inherit the rule. Defines a narrow exception to Privacy Boundary's one-way flow: agents may append reflection entries to `~/Desktop/Gauntlet/KnowledgeBase/weekN/LEARNINGS.md` under a strict three-part trigger. Includes file-target derivation from `_planning/cohort5/CURRENT_WEEK`, entry-shape template (≤120 words), POSIX append mechanic, four failure modes, and a subagent rule. Template version uses an HTML-comment placeholder where each concrete persona appends its lens-specific line.
   - Scope class: `behavioral` (template-level — flows into all future agents)
   - Reason: enable per-session reflection capture by personas in a directional-but-permitted way; preserve Privacy Boundary integrity.

2. **Added directional clarification** to Privacy Boundary section: one-line note that the prohibition governs *KB → git-tracked* outflow only and does not prohibit *agent → KB* writes under Charter Reflection.
   - Scope class: `clarifying` (no behavior change to outflow rule)
   - Reason: prevents conflicting reads of the two rules.

### Rollback pointer

Pre-state SHA: `28e8c04cfe6ae453889a4b4a5afed2305ddc2ada`
