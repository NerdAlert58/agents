# GauntletAI Cohort 5 — Agent Community Planning

**Status:** in design (brainstorming phase)
**Started:** 2026-05-09
**Owner:** user (challenger), with Holdy as design partner
**Expected duration:** multi-day; spec → plan → incremental build over the 8-week bootcamp

This is a **persistent planning document** that lives across sessions. Append to it; do not rewrite.

---

## Context

- User is a challenger in **GauntletAI Cohort 5** — 8-week intensive bootcamp
- Assignments, tasks, and challenges arriving on a "grueling schedule"
- User needs an **agent community** (not a single agent) that can be assembled per assignment
- Multi-agent orchestration is the explicit interaction model — agents may be invited into the same conversation
- This planning conversation may span multiple days

---

## Stated needs (from user, verbatim summary)

1. **Coverage:** complete entire assignments, meet all requirements
2. **Efficiency:** cost-effective and time-efficient methods
3. **Security:** protect from security concerns
4. **Teaching:** explain architecture, design, security, AI, DevOps as needed
5. **Retention:** help retain knowledge across the 8 weeks
6. **TDD discipline:** good tester, good test writer, **great verifier** that won't proceed without:
   - deterministic definition of pass / fail / done
   - strict adherence to "no slop slides by"
7. **Persistent task tracking** for requirements, todos, notes, learnings, suggestions

---

## Open Questions (active — drives the brainstorm)

- [ ] **Q1:** What does a typical bootcamp assignment look like in concrete terms? (deliverable shape, evaluation criteria, time budget per assignment)
- [ ] **Q2:** How will multiple agents collaborate during one assignment? (single Claude Code session with subagents? sequential sessions? parallel sessions? something else?)
- [ ] **Q3:** What's your tech stack / language constraints?
- [ ] **Q4:** What's your current skill profile across the topic areas? (where do you need teaching most?)
- [ ] **Q5:** What's your token / cost budget per assignment, and which models do you have access to?
- [ ] **Q6:** What's your "knowledge retention" mental model? (notes you re-read? quiz-yourself? a wiki you grow?)
- [ ] **Q7:** How do you define "done" for an assignment? (rubric provided by GauntletAI? self-defined? both?)
- [ ] **Q8:** Are there agents you've already imagined that should be in the roster regardless of how design lands?

---

## Decisions (locked-in)

- **2026-05-09:** Planning doc lives at `/Users/nerd/Git/agents/_planning/cohort5-community.md` (in the agents repo, under the `_` convention used by `_templates/`). This is the canonical multi-day notes file for this project.

---

## Role Categories (initial straw man — to be refined after Q&A)

Five tiers, scaled by necessity:

### Tier 1 — Foundation (without these, the system doesn't function)
- **Assignment Intake / Requirements Interpreter** — translates the bootcamp prompt into a structured spec with explicit success criteria
- **Architect** — system design, structural thinking, cross-cutting concerns
- **TDD Trio:**
  - Test Designer — what should be tested, what failure modes to cover
  - Test Writer — actual test code that's clean and maintainable
  - Verifier — refuses to call anything done without deterministic pass/fail criteria; does not let slop slide

### Tier 2 — Quality & Risk (catches what foundation misses)
- Security Reviewer — threat-models the design, audits for common pitfalls
- Code Reviewer — general quality, idioms, readability
- Cost / Efficiency Reviewer — flags expensive patterns, suggests cheaper alternatives

### Tier 3 — Teach & Retain (the meta-goal of the bootcamp)
- Teacher / Explainer — just-in-time concept teaching at the right depth
- Knowledge Curator — captures decisions, gotchas, and concept connections into a growing personal knowledge base

### Tier 4 — Domain Specialists (called as needed, not always active)
- DevOps / Infra
- AI / ML specifics
- Frontend / Backend / Data — as the curriculum demands

### Tier 5 — Flair (optional, for personality and edge-case value)
- Devil's Advocate — steelmans alternatives the team didn't consider
- Bootcamp Historian — tracks the user's trajectory across the 8 weeks; flags patterns ("you've struggled with X three times now")
- Rubber Duck — pure listener, only asks clarifying questions

This taxonomy is a **starting point for discussion**, not a recommendation. Will be revised after Q&A.

---

## Bootcamp Shape (extracted from real assignments — Week 1, Week 2, Week 2 Surprise)

The bootcamp delivers ~weekly assignments with the following shared structure:

### Per-week lifecycle (observed pattern)

| Phase | Output | Notes |
|---|---|---|
| **1. Intake** | Structured spec from the PDF | PDFs are dense (6–12 pages), include hard gates, soft suggestions, pre-search checklists. Distillation matters. |
| **2. Audit** | `AUDIT.md` with categorized findings + 500-word summary | Categories: Security, Performance, Architecture, Data Quality, Compliance |
| **3. User Definition** | `USERS.md` — narrow user, workflow, use cases | Every agent capability must trace back here |
| **4. Architecture** | `ARCHITECTURE.md` with 500-word summary | Defense-grade; you'll be interviewed on it |
| **5. Build (TDD)** | Code in a forked codebase (Week 1+ = OpenEMR fork) | Production-quality, not prototype |
| **6. Eval Suite** | Boolean-rubric test cases (50+ cases by Week 2) | CI-blocking; must catch regressions |
| **7. Cost Analysis** | Dev spend + projected costs at 100/1K/10K/100K users | Architectural changes per scale tier |
| **8. Observability** | Structured logs: tool sequence, latency, tokens, cost | No raw PHI in logs |
| **9. Demo** | 3–5 min video showing key decisions | |
| **10. Deploy** | Publicly accessible app | Required for every checkpoint after MVP |
| **11. Defense** | AI Interview 24hrs after each submission | "You must defend it" — production thinking, failure modes, scale |
| **12. Carryforward** | Knowledge to apply in next week | Bootcamp explicitly compounds; debt doubles |

### Hard gates per checkpoint (Week 1 example)

- Architecture Defense — 24 hours from start
- MVP — Tuesday 11:59pm CT
- Early Submission — Thursday 11:59pm CT
- Final — Sunday noon CT
- AI Interview — 24hrs after each major submission

### Domain constraints (Healthcare track)

- HIPAA / PHI handling
- Demo data only; treat as if BAA in place with LLM provider
- No raw PHI in logs or screenshots
- Multi-user authorization model required (physician, nurse, resident — different access levels)
- Source attribution required on every clinical claim
- Verification layer required between agent output and user

### Surprise mechanic

Mid-week additional requirements drop. Week 2 added a full PHP-to-modern-framework port (Patient Dashboard) on top of the multi-agent ingest work. Schedule must absorb shocks.

### Repeating deliverable shapes

The community will produce these *every week*:
- Per-week directory containing: AUDIT.md, USERS.md, ARCHITECTURE.md, AI_COST_ANALYSIS.md, README, eval/, demo-script.md, interview-prep.md
- Forked codebase work (OpenEMR or whatever next week brings)
- Deployed environment

---

## Refined Role Thinking (post-PDF reading)

The lifecycle above suggests roles aligned to phases. Specifics still TBD pending Q&A, but here's the sharpened thinking:

### Core lifecycle agents (one per recurring phase, called every week)
- **Intake Analyst** — turns the bootcamp PDF into a structured spec with explicit pass/fail criteria, hard gates, soft suggestions, and a question list back to the user. First agent invoked every week.
- **Auditor** — takes a codebase + user goals → produces AUDIT.md with the five required categories
- **User-Definer** — produces USERS.md; pushes back when user definitions are too vague
- **Architect** — produces ARCHITECTURE.md; defends decisions; integrates with audit findings
- **TDD Trio** (still essential per user request):
  - Test Designer — what to test, what failure modes (especially adversarial/edge cases per the bootcamp's "what does your eval suite test that a happy-path demo would not reveal?")
  - Test Writer — clean test code
  - Verifier — refuses to ship without deterministic boolean rubrics; the bootcamp itself demands this
- **Cost Analyst** — produces the per-user-tier cost analysis with architectural implications
- **Documentation Producer** — produces 500-word summaries that lead each required doc; ensures traceability across AUDIT/USERS/ARCHITECTURE
- **Demo Director** — scripts the 3–5 min video; identifies the "one finding that defines your work"
- **Interview Coach** — drills the user on probable interview questions per the assignment's listed topics

### Cross-cutting agents (always-on lens, not per-phase)
- **Security/HIPAA Reviewer** — applies to everything; auth boundaries, PHI handling, audit logging, prompt injection
- **Codebase Navigator** — orients in OpenEMR (or future codebases); "demonstrating you can orient yourself in a large complex system" is itself graded
- **Knowledge Curator** — captures decisions, gotchas, and concept connections into a growing personal knowledge base across weeks (the "compounding" mandate)

### Optional / context-specific
- **Domain Specialist** (Healthcare/Clinical for the current track) — clinical rules, medical terminology, FHIR/OpenEMR specifics
- **DevOps / Deploy** — for hosting, CI gates, deployment patterns

### Flair
- **Devil's Advocate** — pre-interview, asks the hostile questions a hospital CTO would
- **Bootcamp Historian** — tracks trajectory: what you struggled with in Week 1, where it shows up in Week 2, what's likely to bite in Week 3
- **Rubber Duck** — pure listener for when you're stuck

This is now ~12–14 distinct roles. Decomposition will need to address: which are personas, which are skills/subagents, which collapse together.

---

## Notes & Learnings (cross-session)

- **2026-05-09:** Read W1 + W2 + W2-Surprise PDFs. Bootcamp is multi-deliverable production-grade with interview defense. Roles must map to lifecycle phases, not abstract capabilities. Eval-driven CI with boolean rubrics is the bootcamp's own standard — Verifier agent should adopt this verbatim.

---

## Deferred / Punted

*(Empty — populate as items are explicitly bumped to later.)*

---

## Suggestions (Holdy's running list)

- Adopt a "lead investigator" pattern: one agent per assignment owns coordination; the others are summoned as needed. Avoids confused-deputy chaos when 5+ agents are in scope.
- Consider a **rubric-first** workflow: Verifier gates the assignment with a written pass/fail rubric *before* the Architect designs anything. This forces "definition of done" to come first, which kills slop at the source.
- Build the TDD trio as **the first three agents**. They unlock the discipline that gates everything else. Other tiers come after.
